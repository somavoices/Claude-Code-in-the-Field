# Claude Code in the Field
### A Role-Based Training Kit for Engineering Teams

> Six colleagues. One company. Real work. You pick a role and live it.

**Claude Code in the Field** is a hands-on training kit that teaches professionals how to use Claude Code through realistic, role-specific scenarios — all set inside a shared fictional B2B SaaS company. Participants don't learn prompting in the abstract; they *become* a colleague with a real job, a real squad, and a real deadline.

---

## Project Name Suggestions

| Name | Angle |
|---|---|
| **Claude Code in the Field** | Field service metaphor matches the fictional company |
| **Fieldwork** | Short, evocative, role-immersion feel |
| **Claude Code: Live Roles** | Direct, describes the format |
| **The Squad Kit** | Emphasizes the cross-functional team story |

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
└── personas/               ← One folder per role, fully self-contained
    ├── developer/
    ├── data_analytics/
    ├── qa_testing/
    ├── product_manager/
    ├── business_analyst/
    └── ops_support/
```

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

| File | Purpose |
|---|---|
| `CLAUDE.md` | Role identity file — defines the persona's name, squad, tech stack, active projects, working style, and what Claude must never do. This is the primary context file Claude Code reads. |
| `memory.md` | Extended project memory — domain rules, key relationships, system knowledge, and background a new teammate would need to get up to speed |
| `SKILL.md` | Step-by-step walkthrough of one high-value, role-specific task (e.g., writing a migration for a developer, building a dbt model for a data analyst) |
| `prompt_library.md` | 5–6 ready-to-use prompts for the most common daily tasks in that role — copy, paste, adapt |
| `reference_card.md` | One-page quick reference: how to write better prompts, common mistakes, and role-specific tips |
| `safety_checklist.md` | What to verify before acting on Claude's output — role-specific risks, red flags, and sign-off requirements |
| `eval_checklist.md` | How to score Claude's response quality — criteria for good vs. poor output in that role's context |
| `settings.json` | Claude Code permission settings — what tools are allowed, what requires confirmation, automation rules |
| `mcp_config.json` | MCP server configuration — tool connections to GitHub, Jira, Snowflake, filesystem, or database depending on role |
| `agent_instructions.md` | How to run Claude autonomously on multi-step tasks — task templates, delegation boundaries, and review checkpoints *(present in: developer, data_analytics, qa_testing, ops_support)* |

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
3. Copy that folder's `CLAUDE.md` to your working directory (or point Claude Code to it)
4. Open `prompt_library.md` — pick a prompt and run it
5. Check `safety_checklist.md` before acting on any output

For cross-functional exercises, see `team_playbook.md`.
