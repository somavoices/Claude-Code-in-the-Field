# Training Kit Documentation
## Claude Code — Persona-Based Training Artifacts

> **Status: Draft — content under review**

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
├── CLAUDE.md                  ← Shared company context (auto-loaded by all personas)
├── DOCUMENTATION.md           ← This file
├── README.md                  ← Kit overview and getting started guide
├── team_playbook.md           ← How all 6 personas work together
│
├── .claude/                   ← Project-level Claude Code config (auto-loaded)
│   ├── settings.json          ← Universal rules applying to all personas
│   ├── skills/                ← Slash commands available in any session
│   │   ├── write-pr-description/SKILL.md
│   │   ├── write-dbt-model/SKILL.md
│   │   ├── write-cypress-test/SKILL.md
│   │   ├── write-prd/SKILL.md
│   │   ├── write-functional-requirements/SKILL.md
│   │   └── write-runbook/SKILL.md
│   └── agents/                ← Sub-agent definitions (4 personas)
│       ├── developer-alex.md
│       ├── data-analyst-priya.md
│       ├── qa-jordan.md
│       └── ops-morgan.md
│
└── personas/
    ├── developer/
    ├── data_analytics/
    ├── qa_testing/
    ├── product_manager/
    ├── business_analyst/
    └── ops_support/
        │
        ├── CLAUDE.md              ← Who I am and how Claude should behave (auto-loaded)
        ├── memory.md              ← Project knowledge, people, domain rules (on demand)
        ├── settings.json          ← Role-specific permissions and hooks (auto-loaded)
        ├── .mcp.json              ← Tool connections: GitHub, filesystem, search (auto-loaded)
        ├── prompt_library.md      ← 5–6 ready-to-use prompts — human reference
        ├── reference_card.md      ← Quick-reference for prompt writing — human reference
        ├── safety_checklist.md    ← What to check before acting on output — human reference
        └── eval_checklist.md      ← How to score and improve Claude's output — human reference
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

**How it loads:** Automatically — Claude Code reads it whenever launched from any
subfolder of this project.

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

## .claude/ — Project-Level Config

### .claude/settings.json
**What it is:** The project-wide Claude Code settings file. Applies to every persona.

**What it contains:**
- Universal `deny` rules — no force push, no credential writes — enforced for all roles
- Universal `allow` rules — common git read commands available everywhere
- A credential-detection PreToolUse hook shared across all personas

**How it layers:** Claude Code merges settings at multiple levels —
project (`.claude/settings.json`) → persona (`personas/<role>/settings.json`).
The project level sets the floor; each persona adds role-specific rules on top.

---

### .claude/skills/
**What it is:** Slash commands that teach Claude how to perform one high-value task
per role to that team's exact standards.

**How it works:** Each skill lives in `.claude/skills/<name>/SKILL.md` with YAML
frontmatter. Claude Code registers them automatically at startup. Participants invoke
them by typing `/skill-name` — no manual file loading needed.

**Skills in this kit:**

| Slash command | Persona | Task |
|---|---|---|
| `/write-pr-description` | Developer | GitHub PR descriptions (What/How/Test/Risk/Jira) |
| `/write-dbt-model` | Data & Analytics | dbt models (layer rules, templates, schema.yml) |
| `/write-cypress-test` | QA & Testing | Cypress E2E tests (selectors, intercepts, data isolation) |
| `/write-prd` | Product Manager | Feature specs / PRDs (Given/When/Then ACs) |
| `/write-functional-requirements` | Business Analyst | Functional requirements (REQ-ID, shall/should) |
| `/write-runbook` | Ops & Support | Operational runbooks (concrete steps, escalation paths) |

**Why it matters:** Without a skill, Claude writes in its own generic style. With a
skill invoked, it follows the team's specific conventions — right selector strategy
in Cypress, right layer in dbt, right language in requirements. Output is immediately
usable, not a draft requiring heavy editing.

---

### .claude/agents/
**What it is:** Sub-agent definitions for autonomous multi-step task execution.
Each file defines a named agent with its allowed tools, model, and system prompt.

**Agents in this kit:**

| Agent | Persona | Example tasks |
|---|---|---|
| `developer-alex` | Developer | Code audits, migration reviews, test generation across many files |
| `data-analyst-priya` | Data & Analytics | Model audits, schema.yml generation, RAW schema dependency checks |
| `qa-jordan` | QA & Testing | Cypress spec audits, E2E test generation from acceptance criteria |
| `ops-morgan` | Ops & Support | Runbook audits, post-mortem generation, stale runbook identification |

**Why only 4 personas:** Product Manager and Business Analyst produce documents that
require human judgment at every step. Developer, Data, QA, and Ops have well-defined,
automatable multi-step tasks where autonomous execution saves real time.

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

**How it loads:** Automatically — when launched from inside the persona folder, both
the root `CLAUDE.md` and the persona `CLAUDE.md` load. The persona activates by
launching Claude Code from inside that folder.

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

**How it loads:** On demand — participants load it explicitly at the start of a session:
*"Please read memory.md as additional context."* Not auto-loaded to keep CLAUDE.md lean.

**Training use:** Show participants that Claude gives better, more cautious output
after memory.md is loaded. For example: the developer's memory.md mentions the
Feb 14 double-booking incident — Claude will reference this when reviewing
concurrent assignment code.

---

### settings.json (per persona)
**What it is:** Role-specific Claude Code permissions and hooks, layered on top of
the project-level `.claude/settings.json`.

**What it contains:**
- **`permissions.allow`** — git commands, test runners, and tool actions permitted for this role
- **`permissions.deny`** — commands always blocked (e.g., force push, prod DB writes)
- **`hooks`** — shell commands that run automatically at key moments:
  - *PreToolUse hooks* block dangerous actions before they happen (e.g., block any
    SQL with DROP/TRUNCATE, block cy.wait([number]) in test files)
  - *PostToolUse hooks* remind the participant of next steps after a file is written
  - *Stop hooks* run a checklist summary when the session ends

**Why it matters:** settings.json moves safety from "Claude tries to remember the
rules" to "the system enforces the rules automatically." A hook that blocks
`cy.wait(3000)` never forgets to check.

**Training use:** Demonstrate a hook firing — e.g., the Developer trying to run a
DROP statement and the PreToolUse hook blocking it. Explain that hooks are the
difference between Claude as a polite assistant and Claude as a system with
enforced guardrails.

---

### .mcp.json (per persona)
**What it is:** The MCP (Model Context Protocol) server configuration for this
persona — defines which external tools Claude connects to at startup.

**What it contains:** A `mcpServers` object with one entry per tool. Each entry
specifies the `command` to run (e.g., `npx`), `args`, and `env` for credentials.
Permissions and safety rules for those tools are enforced via `settings.json` hooks
— not inside `.mcp.json` itself.

**Tools configured per persona:**

| Persona | Tools |
|---|---|
| Developer | GitHub, PostgreSQL staging DB (read-only), Filesystem, Web search |
| Data & Analytics | Filesystem (dbt project), GitHub (read-only), Web search |
| QA & Testing | Filesystem (test directories), GitHub (read-only), Web search |
| Product Manager | Filesystem (docs), Web search |
| Business Analyst | Filesystem (specs), GitHub (read-only), Web search |
| Ops & Support | Filesystem (runbooks), GitHub (read + PR creation), Web search |

**How it loads:** Automatically — Claude Code reads `.mcp.json` at startup and
connects the configured servers.

**Training use:** Use in Phase 3 (Integration) to demonstrate Claude reading an
actual file from the codebase before generating output. Compare output quality
with and without MCP tool access.

---

### prompt_library.md
**What it is:** A collection of 5–6 complete, ready-to-use prompts for the most
common tasks this persona performs with Claude. **Human reference — do not load into Claude.**

**What makes these prompts effective:**
- Every prompt includes role context, the task, the rules, and placeholders for input
- Rules reflect the team's actual conventions, not generic best practices
- Placeholders are clearly marked — participants fill in the actual content

**Training use:** Have participants run a prompt from the library in the first session
before they've learned any prompt engineering theory. The immediate quality of the
output demonstrates the value of good context and specificity.

---

### reference_card.md
**What it is:** A one-page quick-reference guide for writing effective prompts in
this specific role. Keep open alongside a session. **Human reference — do not load into Claude.**

**What it contains:**
- The 4-part prompt structure — Context, Task, Rules, Input — with role-specific examples
- Vague vs. specific prompt comparisons
- Context shortcuts — pre-written stack descriptions to paste into any prompt
- Iteration patterns — what to do when Claude gets it wrong

**Training use:** Open at the start of Phase 1 exercises. Fastest way to get
participants writing better prompts independently.

---

### safety_checklist.md
**What it is:** A role-specific checklist to run before acting on any Claude output.
**Human reference — do not load into Claude.**

**Structure:**
- Multiple sections per output type (Generated Code, PR Description, SQL Query, etc.)
- Each check is a yes/no question — No means the output needs revision
- Score thresholds (e.g., "6/7 to use; below 6, refine and re-run")
- Red lines — non-negotiable rules regardless of score

**Training use:** After any exercise, run the checklist as a group to build the
evaluation habit.

---

### eval_checklist.md
**What it is:** A structured evaluation framework for scoring Claude's output quality.
**Human reference — do not load into Claude.**

**What it contains:**
- Scored sections — 6–10 checks per output type with Pass / Revise / Reject columns
- Score thresholds and hard rejects
- Iteration log — record what prompt worked, what didn't, and what to change

**Training use:** Central to Phase 3. Run an exercise where participants deliberately
use a weak prompt, score the output, identify the gap, improve the prompt, score again.

---

## How the Files Work Together

### What Loads Automatically vs. On Demand

| File | How it loads |
|---|---|
| `CLAUDE.md` (root + persona) | **Automatic** — on launch from the persona folder |
| `settings.json` (project + persona) | **Automatic** — merged at startup |
| `.mcp.json` | **Automatic** — MCP servers connect at startup |
| `.claude/skills/` | **Automatic** — registered as slash commands |
| `.claude/agents/` | **On demand** — invoked explicitly or spawned by Claude |
| `memory.md` | **On demand** — *"Please read memory.md as context"* |
| `prompt_library.md` | **Human reference** — copy prompts into the session |
| `reference_card.md` | **Human reference** — keep open alongside the session |
| `safety_checklist.md` | **Human reference** — check before acting on output |
| `eval_checklist.md` | **Human reference** — score output quality after a session |

### Session Setup (what a participant does)

```
1. cd personas/<role>         → navigate to persona folder
2. claude                     → launch Claude Code
                                 CLAUDE.md (root + persona) auto-loads
                                 settings.json (project + persona) auto-loads
                                 .mcp.json auto-loads
                                 skills register as slash commands
3. "Read memory.md"           → optional: load deeper project context
4. /skill-name                → invoke a skill for a specific task
5. Write prompt               → use prompt_library.md as reference
```

---

## Training Phase Mapping

| Training Phase | Topic | Files Used |
|---|---|---|
| Phase 1 — Foundation | Prompt engineering | `prompt_library.md`, `reference_card.md` |
| Phase 1 — Foundation | Permissions & safety | `settings.json`, `safety_checklist.md` |
| Phase 2 — Daily Use | Context & memory | `CLAUDE.md` (persona + root), `memory.md` |
| Phase 2 — Daily Use | Skills | `.claude/skills/` slash commands |
| Phase 3 — Integration | Tool use & MCP | `.mcp.json` |
| Phase 3 — Integration | Evaluation & iteration | `eval_checklist.md` |
| Phase 3 — Integration | Cross-team workflows | root `CLAUDE.md`, `team_playbook.md` |
| Phase 4 — Automation | Agents | `.claude/agents/` |
| Phase 4 — Automation | Hooks | `settings.json` hooks section |

---

## Facilitator Notes

### Recommended Session Flow
1. **Open with team_playbook.md** — show all six personas on one page, explain they are colleagues
2. **Load root CLAUDE.md + persona CLAUDE.md** — show the before/after difference in output quality
3. **Run a prompt from prompt_library.md** — immediate success builds confidence
4. **Introduce reference_card.md** — teach the 4-part structure using the example just run
5. **Apply safety_checklist.md** — evaluate the output they just got
6. **Load memory.md** — show how output changes with project-specific knowledge
7. **Invoke a skill** — type `/skill-name`, show output matching team conventions without instruction
8. **Configure .mcp.json** — connect a tool, show Claude reading real files
9. **Run eval_checklist.md** — deliberate iteration exercise
10. **Demo an agent** — invoke a `.claude/agents/` agent, apply post-agent review

### Key Demonstration Contrasts
- Claude without `CLAUDE.md` vs. with it loaded
- Claude without `memory.md` vs. with it (especially incident/lesson references)
- A weak prompt vs. the same task using `prompt_library.md` template
- Output accepted immediately vs. output evaluated with `safety_checklist.md`
- Single-turn task vs. agent mode via `.claude/agents/`
- No hooks vs. a PreToolUse hook blocking a dangerous command live

### What Participants Take Away
Each participant leaves with a complete, working Claude Code setup for their role —
all files configured, prompts ready to use, hooks active, skills registered. They do
not need to build anything from scratch. The training kit is the starting point for
real daily use.
