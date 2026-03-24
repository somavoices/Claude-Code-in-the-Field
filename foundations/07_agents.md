# 07 — Agents
## Running Claude Autonomously on Multi-Step Tasks

---

## What You Will Learn

What an agent is, how it differs from a regular conversation, when to use agent mode,
how to set one up safely, and how to review what it produced.

**Estimated reading time: 20 minutes.**

---

## What Is an Agent?

In a regular Claude Code conversation, you and Claude take turns:
- You write a prompt
- Claude responds
- You read the response and decide what to do next
- You write the next prompt
- And so on

An **agent** is different. You give Claude a goal and a set of constraints,
and Claude works through multiple steps on its own — reading files, writing files,
running checks, making decisions — until the goal is complete or it needs your input.

**The difference in practice:**

*Regular conversation — reviewing one function:*
```
You:    Review this function for N+1 query patterns.
Claude: [Reviews the function, finds one issue]
You:    Now review the next function.
Claude: [Reviews, finds no issues]
You:    Now review the migration file.
Claude: [Reviews, finds missing rollback]
...  (you repeat this 20 times for 20 files)
```

*Agent mode — reviewing all migrations in a folder:*
```
You:    Read every SQL file in db/migrations/ and check each one for:
        missing rollback scripts, missing indexes on foreign keys,
        and DROP/TRUNCATE without a WHERE clause.
        Produce a report listing each file, issues found, and severity.

Claude: [Autonomously reads all 23 migration files, applies checks to each,
         produces a structured report listing 4 issues across 3 files]
```

The second approach takes one prompt instead of twenty, and Claude handles
the iteration internally.

---

## When to Use Agent Mode

Agent mode is valuable for tasks that are:
- **Repetitive** — the same check applied to many files
- **Multi-step** — requires reading first, then writing, then verifying
- **Well-defined** — you can state the success criteria precisely
- **Lower risk** — read-heavy tasks, not write-heavy tasks

**Good use cases by persona:**

| Persona | Good agent tasks |
|---|---|
| Developer | Audit all migrations for missing rollbacks; find all raw db.query() calls in src/ |
| Data & Analytics | Check all fct_ models for missing schema.yml tests; audit staging models for SELECT * |
| QA | Audit all Cypress specs for cy.wait([number]); generate tests for all ACs in a feature |
| Ops | Audit all runbooks for vague steps; update outdated runbooks across a folder |

**Poor use cases (use regular conversation instead):**
- Tasks requiring judgment at each step ("should I use approach A or B?")
- Tasks with unclear success criteria
- Anything that writes to production or sends messages to people
- Novel tasks you have not done before with Claude

---

## How Agent Mode Works Technically

When running in agent mode, Claude Code uses **tools** — the same MCP servers
you configured — to take actions autonomously:

1. Claude reads the task and breaks it into steps
2. For each step, it calls the appropriate tool (read a file, search for a pattern,
   write a file, run a command)
3. It evaluates the result of each tool call and decides the next step
4. It continues until the task is complete or it hits a constraint it cannot pass

The key difference from a regular conversation: **Claude decides the sequence
of steps itself.** You set the goal and the guardrails; Claude figures out the path.

---

## Setting Up an Agent Session Safely

Each persona's `agent_instructions.md` file contains a ready-made agent
constraints prompt. Before using it, understand the three parts:

### Part 1 — Constraints Prompt
This is a system-level instruction you give Claude at the start of the agent session.
It defines:
- Who Claude is acting as (your persona)
- What it is allowed to do (read files, write to feature branches, etc.)
- What it must never do (modify application code, trigger CI, push to main)
- Task execution rules (read before writing, flag not fix anything out of scope)

**Always include the constraints prompt when starting an agent session.**
Without it, Claude will still work autonomously but without your role-specific guardrails.

### Part 2 — Task Handoff Template
A structured format for describing the task:

```
[AGENT TASK]
Goal:               What the agent should accomplish
Scope:              Which files/folders are in scope
Out of scope:       What NOT to touch
Success criteria:   How to know when done
Deliverables:       What the agent should produce
Flag to me if:      Conditions requiring a human decision
```

The "Flag to me if" field is important. It tells Claude when to stop and ask you
rather than making a judgment call. Examples:
- "Flag if you find a migration that touches the dispatch_jobs table — do not include that in scope"
- "Flag if any model has no clear grain definition — do not generate the schema.yml for it"

### Part 3 — Post-Agent Review Checklist
After the agent finishes, do not use its output without reviewing it first.
Each persona's `agent_instructions.md` includes a review checklist specific
to that task type.

---

## A Complete Agent Session Example

Here is what a full agent session looks like for the QA Engineer persona:

**Step 1 — Load context (as always):**
```
> Load personas/qa_testing/CLAUDE.md, memory.md, and SKILL.md as context.
```

**Step 2 — Paste the constraints prompt from agent_instructions.md:**
```
> You are acting as an autonomous QA agent for Jordan Lee, QA Engineer...
  [full constraints prompt from agent_instructions.md]
```

**Step 3 — Give the task using the handoff template:**
```
> [AGENT TASK]
  Goal: Audit all Cypress specs in apps/web/cypress/e2e/dispatch/ for cy.wait([number])
        usage. For each instance found, identify the test, the line, and the
        correct cy.intercept + cy.wait('@alias') replacement.

  Scope: apps/web/cypress/e2e/dispatch/ only
  Out of scope: Do not modify any files — report only

  Success criteria: Every .cy.ts file in the folder has been checked.
                    A report lists: file name, line number, current code, suggested fix.

  Deliverables: A markdown report of all cy.wait([number]) instances found.

  Flag to me if: Any file uses a cy.wait pattern that is not straightforward to replace.
```

**Step 4 — Claude runs autonomously:**
Claude reads each file, searches for the pattern, collects findings, and produces
the report. You wait. It might take 1–3 minutes for a large folder.

**Step 5 — Review with the post-agent checklist:**
```
After agent completes:
- [ ] Did Claude check every file in the scope?
- [ ] Is each finding specific (file name, line number)?
- [ ] Is the suggested replacement correct (cy.intercept + cy.wait)?
- [ ] Did Claude flag anything that needs your judgment?
```

**Step 6 — Act on the output:**
Use the report to prioritise which tests to fix first. Do not start fixing based
on the report alone — verify a few findings manually before trusting the full list.

---

## Common Agent Mistakes

**Giving an agent a task that is too open-ended:**
"Review the whole codebase for issues" — too broad. Claude will spend a lot of
tokens on low-value findings and may miss the high-value ones.

**Not setting an "out of scope" boundary:**
Without explicit out-of-scope boundaries, Claude may read or modify files you
did not intend. Always specify what NOT to touch.

**Not reviewing the output:**
Agent output is still Claude output — it can still hallucinate. A flaky test
audit report might include a false positive. A runbook update might introduce
a vague step. Apply the post-agent review checklist every time.

**Using agent mode for tasks that require judgment:**
"Refactor the dispatch module for better readability" — this requires constant
judgment calls that should involve you. Use regular conversation for tasks
where you need to guide each step.

---

## Agents Are Available for Four Personas

| Has agent_instructions.md | Why |
|---|---|
| Developer ✅ | Code audits, migration reviews, test generation — well-defined, automatable |
| Data & Analytics ✅ | Model audits, schema.yml generation — repetitive, well-defined |
| QA ✅ | Test audits, spec generation from ACs — repetitive, well-defined |
| Ops & Support ✅ | Runbook audits, post-mortem generation — well-defined, high value |
| Product Manager ❌ | PRDs and ACs require constant human judgment — not automatable |
| Business Analyst ❌ | Requirements need stakeholder decisions at every step — not automatable |

---

## Your Next Step

Go to [GLOSSARY.md](GLOSSARY.md) for a quick reference to all the key terms
used in this training kit.

Then you are ready to start Phase 1 of the training — working with your persona's
`prompt_library.md` and `reference_card.md`.
