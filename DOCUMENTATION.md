# Training Kit Documentation
## Claude Code — Persona-Based Training Artifacts

---

## What Is This Kit?

This training kit helps participants across six different job roles learn how to use
Claude Code effectively in their daily work. Each role gets a dedicated folder of files
that Claude loads as context — so Claude behaves like a knowledgeable colleague who
understands your role, your tools, your team, and your constraints.

All six personas work at the **same fictional company**: a mid-size B2B SaaS business
(~300 employees) building a field service management platform. They are colleagues.
They share the same Jira board, the same GitHub org, the same product, and the same
active projects. This makes the training realistic — participants can see how Claude
helps different roles on the same team.

---

## The Six Personas

| Persona | Name | Role | Squad |
|---|---|---|---|
| Developer | Alex Rivera | Full Stack Engineer | Dispatch Core |
| Data & Analytics | Priya Nair | Senior Data Analyst | Data Platform |
| QA & Testing | Jordan Lee | QA Engineer | Dispatch Core |
| Product Manager | Sam Okafor | Product Manager | Dispatch Core |
| Business Analyst | Chris Wang | Business Analyst | Integrations |
| Ops & Support | Morgan Blake | Senior Support Engineer | Platform & Reliability |

---

## Folder Structure

```
claude_code_persona/
│
├── CLAUDE.md                  ← Shared company context (loaded by all personas)
├── DOCUMENTATION.md           ← This file
├── team_playbook.md           ← How all 6 personas work together
│
└── personas/
    ├── developer/
    ├── data_analytics/
    ├── qa_testing/
    ├── product_manager/
    ├── business_analyst/
    └── ops_support/
        │
        ├── CLAUDE.md              ← Who I am and how Claude should behave
        ├── memory.md              ← Project knowledge, people, domain rules
        ├── SKILL.md               ← Step-by-step guide for one key task
        ├── settings.json          ← Permissions, safety rules, automation hooks
        ├── prompt_library.md      ← 5–6 ready-to-use prompts for daily tasks
        ├── reference_card.md      ← Quick-reference for prompt writing
        ├── safety_checklist.md    ← What to check before acting on output
        ├── mcp_config.json        ← Tool connections (GitHub, filesystem, search)
        ├── eval_checklist.md      ← How to score and improve Claude's output
        └── agent_instructions.md  ← How to run Claude autonomously (4 roles only)
```

---

## Root-Level Files

### CLAUDE.md (root)
**What it is:** The shared company context file that applies to all six personas.

**What it contains:**
- Company description — B2B SaaS, field service management platform, ~300 employees
- All six colleagues listed by name, role, and squad
- Shared systems — GitHub, Jira, Confluence, Datadog, Snowflake, Zendesk, etc.
- Shared active projects — Project Falcon, Enterprise API v2, Customer Health Score
- Cross-team constraints — production access rules, PII policy, external content review

**Why it matters:** Without this file, each persona would only know about their own
work. With it, Alex (Developer) knows that Morgan (Ops) maintains the Datadog alert
rules and must be notified before log format changes. Priya (Data) knows that Alex
owns the source tables her dbt models depend on. The six personas become a team,
not six isolated individuals.

**When to use:** Load this file at the start of every training session, regardless
of which persona is being demonstrated.

---

### team_playbook.md
**What it is:** A cross-persona workflow guide showing how all six roles use Claude
together to deliver a feature from discovery to production.

**What it contains:**
- The full feature delivery workflow — from Sam's PRD to Alex's code to Jordan's tests
  to Morgan's runbooks to Priya's dashboards
- Handoff points — who depends on who, and what Claude's role is at each handoff
- Cross-persona prompt chains — e.g., Sam's acceptance criteria fed directly into
  Jordan's test generation prompt
- Principles for what Claude does well vs. what it should never do autonomously

**Why it matters:** Most training sessions focus on one persona at a time. This file
gives facilitators the material to explain the bigger picture: how Claude multiplies
value across an entire team, not just for one individual.

**When to use:** Use in the opening session to set context, or in a capstone session
where participants from multiple roles collaborate on the same feature.

---

## Per-Persona Files

Each of the following files exists in every persona folder. The content is specific
to that persona's role, tools, and context — but the structure is consistent.

---

### CLAUDE.md (per persona)
**What it is:** The primary context file that tells Claude who this person is,
what they are working on, and how it should behave.

**What it contains:**
- **Who I am** — name, role, squad, tech stack, colleague names
- **Active work** — current projects with Jira ticket numbers, deadlines, open decisions
- **How Claude should behave** — tone, code/writing conventions, format rules
- **What Claude must never do** — role-specific red lines (e.g., never push to main,
  never publish without review, never run dbt on prod without approval)

**Why it matters:** This is the most important file in the kit. When a participant
loads this into Claude, it stops behaving generically and starts behaving like a
colleague who knows their exact situation. The quality of Claude's output increases
immediately because it understands the context without needing to be told every time.

**Training use:** Demonstrate the difference between prompting Claude with and without
this file loaded. The contrast makes the value of CLAUDE.md immediately obvious.

---

### memory.md
**What it is:** Claude's project memory — the accumulated knowledge about the team's
work that Claude would not otherwise know.

**What it contains:**
- **Project context** — current status, branch names, constraints, deadlines
- **Domain rules** — business logic specific to the company (e.g., job statuses,
  monetary values in cents, timestamps always UTC)
- **People and relationships** — who to loop in for what, how each colleague
  prefers to receive information, who owns which decisions
- **Incidents and lessons** — past failures and what was learned, so Claude
  doesn't recommend approaches that have already failed

**Why it matters:** Claude's training data ends at a cutoff date. It knows nothing
about this team's specific incidents, their current sprint, or the race condition
in the job_assignment module. memory.md bridges that gap. It gives Claude the
institutional knowledge that normally only lives in people's heads.

**Training use:** Show participants that Claude gives better, more cautious output
after memory.md is loaded. For example: the developer's memory.md mentions the
Feb 14 double-booking incident — Claude will reference this when reviewing
concurrent assignment code.

---

### SKILL.md
**What it is:** A detailed, executable guide for one high-value task that this
persona does regularly. It teaches Claude exactly how to perform that task
to the team's standards.

**What each persona's SKILL covers:**

| Persona | Skill |
|---|---|
| Developer | Writing GitHub PR descriptions |
| Data & Analytics | Writing dbt models (layer conventions, templates, tests) |
| QA & Testing | Writing Cypress E2E tests (selectors, intercepts, data isolation) |
| Product Manager | Writing feature specs / PRDs (Given/When/Then ACs) |
| Business Analyst | Writing functional requirements (REQ-ID format, shall/should language) |
| Ops & Support | Writing operational runbooks (concrete steps, escalation paths) |

**Why it matters:** Without SKILL.md, Claude writes code or documents in its own
generic style. With SKILL.md loaded, it follows the team's specific conventions —
using the right selector strategy in Cypress tests, the right layer in dbt models,
the right language in requirements. Output is immediately usable, not just a draft
that needs heavy editing.

**Training use:** Compare Claude's output on a test generation task with and without
SKILL.md. The difference demonstrates why encoding your team's conventions into
Claude's context saves significant rework.

---

### settings.json
**What it is:** The configuration file for Claude Code — defines what Claude is
allowed to do, what it must escalate, and for three personas (Developer, QA, Ops),
what automated checks run at key moments.

**What it contains:**
- **Permissions** — which files Claude can read/write, which git branches are allowed,
  which database operations require a second sign-off
- **Escalation triggers** — conditions that require a human decision before proceeding
  (e.g., any change to the dispatch_events schema must notify Priya Nair)
- **Safety guardrails** — hard rules Claude must never break, regardless of instruction
- **Hooks** (Developer, QA, Ops only) — shell commands that run automatically:
  - *PreToolUse hooks* block dangerous actions before they happen (e.g., block any
    SQL with DROP/TRUNCATE, block cy.wait([number]) in test files)
  - *PostToolUse hooks* remind the participant of next steps after a file is written
  - *Stop hooks* run a checklist summary when the session ends

**Why it matters:** settings.json moves safety from "Claude tries to remember the
rules" to "the system enforces the rules automatically." A hook that blocks
`cy.wait(3000)` never forgets to check. A hook that warns about internal service
names in Zendesk drafts catches errors before the customer sees them.

**Training use:** Demonstrate a hook firing — e.g., show the Developer trying to
run a DROP statement and the PreToolUse hook blocking it. Explain that hooks are
the difference between Claude as a polite assistant and Claude as a system with
enforced guardrails.

---

### prompt_library.md
**What it is:** A collection of 5–6 complete, ready-to-use prompts for the most
common tasks this persona performs with Claude.

**What makes these prompts effective:**
- Every prompt includes the role context, the task, the rules, and placeholders
  for the input — the full 4-part structure
- Rules in each prompt reflect the team's actual conventions (not generic best practices)
- Each prompt specifies exactly what to check for, what format to use, and
  what to never do
- Placeholders are clearly marked — participants just fill in the actual content

**Example — Developer's code review prompt covers:**
TypeScript strict violations, N+1 query patterns, missing error handling, hardcoded
values, monetary float storage, missing tests, downstream impact on Mobile squad
and Data Platform.

**Why it matters:** Participants don't need to learn prompt engineering from scratch.
They start with prompts that already work for their role and refine from there.
The prompt library also teaches by example — reading a well-structured prompt
helps participants understand what makes a prompt effective.

**Training use:** Have participants run a prompt from the library in the first session
before they've learned any prompt engineering theory. The immediate quality of the
output demonstrates the value of good context and specificity.

---

### reference_card.md
**What it is:** A one-page quick-reference guide for writing effective prompts in
this specific role. Designed to stay open during training exercises.

**What it contains:**
- **The 4-part prompt structure** — Context, Task, Rules, Input — with role-specific
  examples of each
- **Vague vs. specific prompt examples** — side-by-side comparisons showing how
  specificity improves output quality
- **Context shortcuts** — pre-written stack and convention descriptions to paste
  into any prompt (so participants don't have to re-type them each time)
- **Claude for daily work** — a quick table mapping task type to prompt approach
- **Iteration patterns** — what to do when Claude gets it wrong (too generic,
  wrong style, missing edge case, etc.)
- **Safety reminders** — the top 3–4 things never to paste into a prompt
  (secrets, PII, production credentials)

**Why it matters:** In training, participants often don't know why their prompt
produced a poor result. The reference card gives them a structured way to diagnose
the problem and fix it without needing to ask the facilitator.

**Training use:** Hand this out (or have participants open it) at the start of
Phase 1 exercises. It is the fastest way to get participants writing better prompts
independently.

---

### safety_checklist.md
**What it is:** A role-specific checklist to run before acting on any Claude output.
Covers code quality, data safety, process compliance, and absolute red lines.

**Structure:**
- Multiple sections, each covering one type of output (e.g., Generated Code,
  PR Description, SQL Query)
- Each check is a yes/no question — answer No means the output needs revision
- Score thresholds — e.g., "6/7 to use output; if below 6, refine the prompt and re-run"
- **Red lines section** — non-negotiable rules that apply regardless of score
  (e.g., never push to main, never output PII, never send Zendesk reply without review)

**Why it matters:** Training participants to evaluate Claude's output critically is
as important as training them to prompt effectively. Without an evaluation habit,
participants either over-trust Claude (accepting low-quality output) or under-trust
it (rejecting good output and doing the work manually). The checklist builds the
habit of structured evaluation.

**Training use:** After any exercise that produces Claude output, run the checklist
as a group. The scoring and threshold system makes it concrete — "this output scores
5/7, which is below threshold, so we need to refine the prompt."

---

### mcp_config.json
**What it is:** The MCP (Model Context Protocol) server configuration for this
persona — defines which external tools Claude can connect to, what it is allowed
to do with each tool, and usage guidance.

**What tools are configured per persona:**

| Persona | Tools |
|---|---|
| Developer | GitHub (read + PR creation), PostgreSQL staging DB (read-only), Filesystem, Web search |
| Data & Analytics | Filesystem (dbt project), GitHub (read-only), Web search |
| QA & Testing | Filesystem (test directories), GitHub (read-only), Web search |
| Product Manager | Filesystem (docs), Web search |
| Business Analyst | Filesystem (specs), GitHub (read-only), Web search |
| Ops & Support | Filesystem (runbooks), GitHub (read + PR creation), Web search |

**Why it matters:** MCP is how Claude goes from a text assistant to an active
participant in the workflow — reading actual code files, searching documentation,
querying the database to verify queries. The config file shows participants how
to give Claude real access to their tools while maintaining appropriate boundaries
(e.g., GitHub read is always allowed; merging PRs is never allowed).

**Training use:** Use in Phase 3 (Integration) to demonstrate Claude reading an
actual file from the codebase before generating output. Compare output quality
with and without MCP tool access.

---

### eval_checklist.md
**What it is:** A structured evaluation framework for scoring Claude's output
quality, with pass/revise/reject criteria and a score threshold for each output type.

**What it contains:**
- **Scored sections** — one per output type (e.g., Generated Code, PR Description,
  SQL Query). Each has 6–10 specific checks with Pass / Revise / Reject columns.
- **Score thresholds** — e.g., "7/8 to use output. Any monetary float = reject immediately."
- **Hard rejects** — specific conditions that mean reject regardless of overall score
  (e.g., any PII in output, any blame language in a post-mortem)
- **Iteration log** — a table for recording what prompt worked, what didn't, and
  what to change next time

**Why it matters:** This file teaches the evaluation-and-iteration loop, which is
the skill that separates effective Claude users from ineffective ones. Good prompting
is not about getting perfect output the first time — it is about quickly identifying
what is wrong and improving the prompt. The iteration log makes this a habit, not
a one-off.

**Training use:** Central to Phase 3 (Evaluation & Iteration). Run an exercise where
participants deliberately use a weak prompt, score the output with the checklist,
identify the gap, improve the prompt, and score again.

---

### agent_instructions.md
*(Developer, Data & Analytics, QA & Testing, Ops & Support only)*

**What it is:** A guide for running Claude as an autonomous agent — delegating a
multi-step task and letting Claude work through it with minimal intervention.

**What it contains:**
- **When to use agent mode** — concrete examples of tasks suitable for autonomous
  execution (e.g., "audit all Cypress specs for cy.wait([number]) usage")
- **Agent constraints prompt** — a complete system prompt to give Claude when
  starting an agent session. Defines: squad context, permissions, task execution
  rules, and absolute never-dos. Copy-paste ready.
- **Task handoff template** — a structured format for specifying Goal, Scope,
  Out of scope, Success criteria, Deliverables, and Flag conditions
- **Post-agent review checklist** — what to verify after the agent completes,
  before using or merging its output

**Why this is only for 4 personas:** Product Manager and Business Analyst roles
produce documents that require human judgment at every step — a PM should not
autonomously create Jira tickets or publish specs. Developer, Data, QA, and Ops
have well-defined, automatable multi-step tasks (code audit, model generation,
test generation, runbook audit) where autonomous execution saves real time.

**Why it matters:** Agent mode is the highest-leverage use of Claude — it handles
tasks that would otherwise take hours. But it requires clear constraints to be safe.
This file teaches participants both when to use it and how to set it up correctly.

**Training use:** Phase 4 (Automation). Demonstrate an agent session live —
e.g., the Developer agent auditing all migrations for missing rollback scripts.
Show the task handoff template being filled in, the agent running, and the
post-agent review checklist being applied.

---

## How the Files Work Together

### Loading Order for a Session

```
Step 1: Load root CLAUDE.md          → Claude knows the company, team, shared projects
Step 2: Load persona CLAUDE.md       → Claude knows who you are and your constraints
Step 3: Load persona memory.md       → Claude knows your project history and domain rules
Step 4: Load persona SKILL.md        → Claude knows how to do your key task correctly
```

Once these four files are loaded, Claude behaves as a role-specific colleague.
The remaining files are tools the *participant* uses — not loaded into Claude.

### Files the Participant Uses (not loaded into Claude)

| File | When the participant uses it |
|---|---|
| `prompt_library.md` | To find a ready-made prompt for the task at hand |
| `reference_card.md` | To craft or improve a prompt from scratch |
| `safety_checklist.md` | Before acting on Claude's output |
| `eval_checklist.md` | To score output quality and decide whether to iterate |
| `mcp_config.json` | To configure tool connections at session setup |
| `agent_instructions.md` | When delegating a multi-step task to Claude |
| `settings.json` | Configured once at Claude Code setup — hooks run automatically |

---

## Training Phase Mapping

| Training Phase | Topic | Files Used |
|---|---|---|
| Phase 1 — Foundation | Prompt engineering | `prompt_library.md`, `reference_card.md` |
| Phase 1 — Foundation | Best practices | `reference_card.md` |
| Phase 1 — Foundation | Permissions & safety | `settings.json`, `safety_checklist.md` |
| Phase 2 — Daily Use | Daily work patterns | `reference_card.md` (daily work section) |
| Phase 2 — Daily Use | Memory | `memory.md` |
| Phase 2 — Daily Use | Skills | `SKILL.md` |
| Phase 2 — Daily Use | CLAUDE.md | `CLAUDE.md` (persona + root) |
| Phase 3 — Integration | Tool use & MCP | `mcp_config.json` |
| Phase 3 — Integration | Co-work | Root `CLAUDE.md`, `team_playbook.md` |
| Phase 3 — Integration | Evaluation & iteration | `eval_checklist.md` |
| Phase 4 — Automation | Agents | `agent_instructions.md` |
| Phase 4 — Automation | Hooks | `settings.json` (hooks section) |
| Phase 4 — Automation | Overall work patterns | `team_playbook.md` |

---

## Facilitator Notes

### Recommended Session Flow
1. **Open with the team_playbook.md** — show all six personas on one page, explain
   they are colleagues at the same company
2. **Load root CLAUDE.md + persona CLAUDE.md** — show the before/after difference
   in Claude's output quality
3. **Run a prompt from prompt_library.md** — immediate success builds confidence
4. **Introduce reference_card.md** — teach the 4-part structure using the example
   the participant just ran
5. **Apply safety_checklist.md** — evaluate the output they just got
6. **Load memory.md** — show how Claude's output changes with project-specific knowledge
7. **Load SKILL.md** — show how output matches team conventions without instruction
8. **Configure mcp_config.json** — connect a tool and show Claude reading real files
9. **Run eval_checklist.md** — deliberate iteration exercise
10. **Demo agent_instructions.md** — run an autonomous task, apply post-agent review

### Key Demonstration Contrasts
- Claude without CLAUDE.md vs. with CLAUDE.md loaded
- Claude without memory.md vs. with memory.md (especially incident/lesson references)
- A weak prompt vs. the same task using prompt_library.md template
- Output accepted immediately vs. output evaluated with safety_checklist.md
- Single-turn task vs. agent mode with agent_instructions.md

### What Participants Take Away
Each participant leaves with a complete, working Claude Code setup for their role —
all files configured, prompts ready to use, hooks active. They do not need to build
anything from scratch. The training kit is the starting point for real daily use.
