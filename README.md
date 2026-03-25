# Claude Code in the Field
### A Role-Based Training Kit for Engineering Teams

> **Status: Draft — content under review** — Some sections may be incomplete or subject to change. Not yet ready for facilitator-led delivery.

> Six colleagues. One company. Real work. You pick a role and live it.

**Claude Code in the Field** is a hands-on training kit that teaches professionals how to use Claude Code through realistic, role-specific scenarios — all set inside a shared fictional B2B SaaS company. Participants don't learn prompting in the abstract; they *become* a colleague with a real job, a real squad, and a real deadline.

---

## Who This Is For

Teams and individuals in any of these roles who want to learn Claude Code through *their own work*, not generic tutorials:

- Developers
- Data analysts
- QA engineers
- Product managers
- Business analysts
- Ops / support engineers

---

## How It Works

Each participant is assigned a **persona folder**. Their persona's `CLAUDE.md` becomes their identity — Claude Code reads it and behaves as their informed, role-aware teammate. Every task they run is grounded in real squad work at the fictional company.

The kit is structured in three layers:

```
claude_code_persona/
├── CLAUDE.md               ← Shared company context (loaded by all personas)
├── README.md               ← This file
├── DOCUMENTATION.md        ← Index of all guides and materials
├── team_playbook.md        ← How the six roles hand off work to each other
├── foundations/            ← Progressive curriculum (concept → agent autonomy)
├── .claude/                ← Project-level Claude Code config (skills, agents)
│   ├── skills/             ← Slash commands available across all personas
│   └── agents/             ← Sub-agent definitions for autonomous tasks
└── personas/               ← One folder per role, fully self-contained
    ├── developer/
    ├── data_analytics/
    ├── qa_testing/
    ├── product_manager/
    ├── business_analyst/
    └── ops_support/
```

---

## How CLAUDE.md Loading Works

**Claude Code loads `CLAUDE.md` automatically by directory — not by persona name.**

It reads every `CLAUDE.md` found walking up the directory tree from where you launch it.

| Launch from | What loads automatically |
|---|---|
| `claude_code_persona/` | Root `CLAUDE.md` only — shared company context, no persona |
| `claude_code_persona/personas/developer/` | Root `CLAUDE.md` + `developer/CLAUDE.md` — Alex persona active |
| `claude_code_persona/personas/qa_testing/` | Root `CLAUDE.md` + `qa_testing/CLAUDE.md` — Jordan persona active |

**The persona activates by launching Claude Code from inside that persona's folder.** Switch persona by switching the directory you launch from.

### What loads automatically vs. on demand

| File | How it loads |
|---|---|
| `CLAUDE.md` | **Automatic** — loaded on launch if in the directory path |
| `settings.json` | **Automatic** — all layers merged at startup (see below) |
| `.mcp.json` | **Automatic** — MCP servers connect at startup |
| `.claude/skills/` | **Automatic** — registered as slash commands (e.g. `/write-pr-description`) |
| `.claude/agents/` | **On demand** — invoked explicitly or spawned by Claude for subtasks |
| `memory.md` | **On demand** — load it when you need deeper project context: *"Read memory.md before we begin"* |
| `prompt_library.md` | **Human reference** — copy prompts from here into your session; do not load into Claude |
| `reference_card.md` | **Human reference** — keep open alongside your session; do not load into Claude |
| `eval_checklist.md` | **Human reference** — use after a session to grade Claude's output quality |
| `safety_checklist.md` | **Human reference** — check before acting on Claude's output |

### settings.json layers

`settings.json` loads at multiple levels and merges — each layer adds to the one above. `deny` rules always win regardless of layer.

| Layer | File | Contains |
|---|---|---|
| User | `~/.claude/settings.json` | Personal defaults across all projects |
| Project | `.claude/settings.json` ← **this kit** | Rules that apply to every persona — no force push, no secrets |
| Local | `.claude/settings.local.json` | Personal overrides, gitignored — not in this kit |
| Persona | `personas/<role>/settings.json` | Role-specific hooks and permissions layered on top |

**In this kit:** the project-level `.claude/settings.json` holds universal constraints (block force push, block credential writes). Each persona's `settings.json` adds role-specific hooks — e.g., the developer persona blocks `DROP/TRUNCATE`, the QA persona blocks `cy.wait([number])`, the ops persona blocks all production write commands.

### When to load memory.md

`memory.md` contains deeper project knowledge that would bloat `CLAUDE.md` if always loaded: past incidents, domain rules, system URLs, relationship notes. Load it when:

- Starting a session on a complex task: *"Read memory.md, then help me with..."*
- Claude is missing context about a person, system, or past decision
- You're working across multiple files and want Claude to have full background

**Multi-layer memory** — if you had a root-level `memory.md` (shared team context) and a persona-level `memory.md` (role-specific context), you can load both: *"Read the root memory.md and personas/developer/memory.md before we begin."* Claude merges both into its working context. This kit keeps memory at the persona level only — shared context lives in the root `CLAUDE.md`.

---

## File Reference

### Root

| File | Purpose |
|---|---|
| `CLAUDE.md` | Shared company context loaded into every session — defines the fictional company, all six personas, active Q2 projects, and shared constraints |
| `DOCUMENTATION.md` | Master index of all guides, personas, and materials in the kit |
| `team_playbook.md` | Cross-functional workflow map showing how work flows between roles (PM → Dev → QA → Ops → Data) |

---

### foundations/

Progressive curriculum. Read in order or jump to what you need.

| File | Purpose |
|---|---|
| `00_getting_started.md` | What Claude Code is, how the kit works, how to orient yourself |
| `01_installation_setup.md` | Installing Claude Code, authenticating, configuring your environment |
| `02_core_concepts.md` | Mental model: how Claude Code reads context, uses tools, and takes action |
| `03_tools_and_mcp.md` | Built-in tools (Read, Edit, Bash, Glob, Grep) and MCP server connections |
| `04_your_first_prompt.md` | Writing your first real prompt — structure, specificity, and context |
| `05_safety_and_risk.md` | What Claude can get wrong, how to verify output, when not to trust it |
| `06_hooks_and_automation.md` | Automating repeated actions with hooks and shell triggers |
| `07_agents.md` | Running Claude autonomously on multi-step tasks; when to delegate vs. supervise |
| `GLOSSARY.md` | Definitions for all terms used across the kit |

---

### personas/

Each persona folder is fully self-contained. A participant can drop into any folder and immediately have everything they need.

#### File Descriptions (same structure in every persona folder)

| File | Loaded by | Purpose |
|---|---|---|
| `CLAUDE.md` | Automatic | Role identity — persona name, squad, stack, active projects, working style, and hard constraints. The primary context file. |
| `memory.md` | On demand | Extended project memory — domain rules, key relationships, past incidents, system URLs. Load explicitly at session start for complex tasks. |
| `settings.json` | Automatic | Claude Code permissions and hooks — controls what commands are allowed and what triggers automated safety checks. |
| `.mcp.json` | Automatic | MCP server connections — GitHub, filesystem, database, or search tools depending on the role. |
| `prompt_library.md` | Human reference | 5–6 copy-paste prompts for the most common daily tasks in that role. Do not load into Claude — use as a reference to compose your prompts. |
| `reference_card.md` | Human reference | One-page prompt engineering guide for that role — 4-part prompt structure, vague vs. specific examples, iteration patterns. Keep open alongside your session. |
| `safety_checklist.md` | Human reference | What to verify before acting on Claude's output — role-specific risks, red flags, and sign-off requirements. |
| `eval_checklist.md` | Human reference | How to score Claude's output quality — criteria for good vs. poor responses in that role's context. |

#### .claude/ (project-level, shared across all personas)

| Path | Loaded by | Purpose |
|---|---|---|
| `.claude/skills/<name>/SKILL.md` | Automatic | Slash commands for role-specific tasks (e.g. `/write-pr-description`, `/write-dbt-model`). Available in any persona session. |
| `.claude/agents/<name>.md` | On demand | Sub-agent definitions for autonomous multi-step tasks. Present for: developer, data_analytics, qa_testing, ops_support. PM and BA excluded by design — their work requires human judgement at every step. |

---

### personas/ — The Six Roles

| Folder | Persona | Role | Squad |
|---|---|---|---|
| `developer/` | Alex Rivera | Full Stack Engineer | Dispatch Core |
| `data_analytics/` | Priya Nair | Senior Data Analyst | Data Platform |
| `qa_testing/` | Jordan Lee | QA Engineer | Dispatch Core |
| `product_manager/` | Sam Okafor | Product Manager | Dispatch Core |
| `business_analyst/` | Chris Wang | Business Analyst | Integrations |
| `ops_support/` | Morgan Blake | Senior Support Engineer | Platform & Reliability |

---

## The Fictional Company

All personas work at the same mid-size B2B SaaS company (~300 employees) building a **field service management platform** — scheduling, dispatch, and tracking for field technicians. Series C, ~60 engineers across 8 squads.

**Active Q2 Projects:**

| Project | Owner(s) | Target |
|---|---|---|
| Project Falcon — Real-Time Dispatch Redesign | Alex, Jordan, Sam, Chris | May 30 |
| Customer Health Score Dashboard | Priya | End of Q2 |
| Enterprise API v2 | Chris, Alex, Morgan | June 15 |

**Shared constraints apply to all personas** — no direct prod DB writes without sign-off, no PII in Slack or Jira, all external copy goes through Sam before publish.

---

## Getting Started

1. Read `foundations/00_getting_started.md`
2. Pick a persona folder that matches your role
3. Launch Claude Code from inside that persona's folder — both `CLAUDE.md` files load automatically
4. Open `prompt_library.md` alongside your session — pick a prompt, replace the placeholders, run it
5. Check `safety_checklist.md` before acting on any output

For cross-functional exercises, see `team_playbook.md`.
