# Glossary
## Key Terms for Claude Code Training

> **Status: Draft — content under review**

Quick reference for every term used in this training kit.
For full explanations, see the linked foundation documents.

---

## A

**Agent**
A Claude Code session where Claude works autonomously through multiple steps
to complete a goal, rather than waiting for you to prompt each step. Used for
repetitive, well-defined multi-step tasks like auditing many files or generating
tests from a list of acceptance criteria. → See [07_agents.md](07_agents.md)

**Agent instructions**
A markdown file in `.claude/agents/` that defines a named sub-agent — its allowed tools,
model, and system prompt (constraints, permissions, task rules). Agents are available for
Developer, Data & Analytics, QA, and Ops personas. Invoked explicitly or spawned
by Claude for multi-step subtasks.

**API key**
A secret credential that authenticates your Claude Code session with Anthropic's
servers. Never paste your API key into a prompt or commit it to a code repository.
Store it as an environment variable.

---

## C

**Claude**
An AI assistant made by Anthropic. Understands and generates text, code, and
structured documents. Does not have internet access in a standard session and
does not retain memory between sessions.

**Claude Code**
Anthropic's command-line tool that integrates Claude into your terminal and
work environment. Unlike the web interface, Claude Code can connect to your
files, repositories, and databases via MCP.

**CLAUDE.md**
A markdown file that provides Claude with context about who you are, what you
are working on, your team's conventions, and your absolute constraints. Loaded
at the start of each session. There is one at the project root (shared company
context) and one per persona (role-specific context). → See [02_core_concepts.md](02_core_concepts.md)

**Context**
All the information Claude has available when generating a response — your
conversation history, files you have asked it to read, and anything loaded
via MCP tools. More specific, relevant context produces better output.
→ See [02_core_concepts.md](02_core_concepts.md)

**Context window**
The maximum amount of text (measured in tokens) Claude can hold in memory
at one time. If a conversation becomes very long, earlier content may be
pushed out. Start a fresh session when context is full.

---

## E

**Eval checklist** (`eval_checklist.md`)
A scored checklist used to evaluate Claude's output quality before using it.
Each section has pass/revise/reject criteria and a score threshold. Includes
an iteration log for tracking what prompts worked. → See [DOCUMENTATION.md](../DOCUMENTATION.md)

---

## H

**Hallucination**
When Claude produces text that sounds correct and confident but is factually
wrong. Can occur in code (wrong method names), SQL (wrong column names),
requirements (wrong business rules), or any other output. Always verify
Claude's output against your actual system before using it.
→ See [05_safety_and_risk.md](05_safety_and_risk.md)

**Hook**
A shell command configured in `settings.json` that runs automatically at
specific moments during a Claude Code session. PreToolUse hooks can block
dangerous actions. PostToolUse hooks add reminders. Stop hooks run a final
checklist when the session ends. → See [06_hooks_and_automation.md](06_hooks_and_automation.md)

---

## I

**Iteration**
The process of running a prompt, evaluating the output, identifying a specific
gap, adding that gap as a constraint to the prompt, and running again. Two to
three iterations is normal for complex tasks. Not a sign of failure — a sign
of good practice.

---

## M

**MCP (Model Context Protocol)**
A standard that allows Claude Code to connect to external tools like your
filesystem, GitHub, databases, and web search. Each connection is called an
MCP server. You configure which operations are allowed and which are denied.
→ See [03_tools_and_mcp.md](03_tools_and_mcp.md)

**MCP server**
One configured tool connection in your `.mcp.json`. Examples: filesystem
server (reads/writes local files), GitHub server (reads repos and PRs), PostgreSQL
server (read-only database queries), Brave Search server (web search).

**memory.md**
A markdown file containing project knowledge that Claude would not otherwise know:
current project status, domain rules, people and relationships, past incidents and
lessons learned. Loaded at the start of each session to give Claude "institutional
memory." → See [02_core_concepts.md](02_core_concepts.md)

---

## P

**Persona**
One of the six role-based configurations in this training kit (Developer, Data &
Analytics, QA, Product Manager, Business Analyst, Ops & Support). Each persona
has a dedicated folder with context files tailored to that role's work, tools,
and constraints.

**Permissions**
Rules in `settings.json` that define what Claude is allowed to do and what
requires human approval or escalation. Examples: allowed git branches, allowed
database operations, files Claude can and cannot read.

**PII (Personally Identifiable Information)**
Any data that identifies a real person: names, email addresses, phone numbers,
account IDs, location data. Never paste PII into Claude prompts — it is sent
to an external API.

**PostToolUse hook**
A hook that runs after Claude executes a tool action. Cannot block the action
but can print reminders of next steps. → See [06_hooks_and_automation.md](06_hooks_and_automation.md)

**PreToolUse hook**
A hook that runs before Claude executes a tool action. Can block the action
entirely if it detects a dangerous pattern. The most important hook type.
→ See [06_hooks_and_automation.md](06_hooks_and_automation.md)

**Prompt**
The message you send to Claude. An effective prompt includes four parts:
Context (who you are and what system is involved), Task (what you want produced),
Rules (your team's standards and constraints), and Input (the actual data to work with).
→ See [02_core_concepts.md](02_core_concepts.md)

**Prompt library** (`prompt_library.md`)
A collection of 5–6 complete, ready-to-use prompts for the most common tasks
each persona performs. Each prompt includes the 4-part structure with role-specific
rules already filled in. Participants can use these directly or adapt them.

---

## R

**Reference card** (`reference_card.md`)
A one-page quick-reference guide for writing prompts in a specific role. Contains
the 4-part prompt structure with role-specific examples, context shortcuts,
a daily work table, iteration patterns, and safety reminders. Designed to stay
open during training exercises.

---

## S

**Safety checklist** (`safety_checklist.md`)
A role-specific checklist to run before acting on Claude's output. Covers
code quality, data safety, process compliance, and absolute red lines. Scored
sections with pass/revise/reject criteria.

**Session**
One Claude Code conversation from start to finish. Claude remembers everything
said within a session but forgets it all when the session ends. Context files
must be reloaded at the start of each new session.

**settings.json**
The configuration file for Claude Code. Contains `permissions` (allow/deny arrays)
and `hooks` (PreToolUse, PostToolUse, Stop) that run automatically. Loads at multiple
levels — project-level (`.claude/settings.json`) applies to all personas; persona-level
(`personas/<role>/settings.json`) adds role-specific rules on top.

**Skill**
A named slash command that teaches Claude how to perform one high-value task to your
team's standards — the exact format, steps, conventions, and anti-patterns. Skills
live in `.claude/skills/<name>/SKILL.md` with YAML frontmatter and are invoked as
`/skill-name` in any session. No manual loading needed.
→ See [02_core_concepts.md](02_core_concepts.md)

**Stop hook**
A hook that runs when a Claude Code session ends. Used to display a final
checklist reminder before the session closes.
→ See [06_hooks_and_automation.md](06_hooks_and_automation.md)

---

## T

**Token**
The basic unit Claude uses to process text — roughly 4 characters or 0.75 words.
Token count determines how much fits in the context window and affects API cost.
A typical page of text is about 500 tokens.

**Tool** (in Claude Code context)
An action Claude can take beyond generating text — reading a file, writing a file,
running a shell command, querying a database. Tools are enabled via MCP configuration.

---

## W

**Workflow map**
The daily and weekly pattern of how a persona uses Claude Code — which tasks to
delegate, which prompts to use, and when to apply each tool in the kit. Found
in each persona's `reference_card.md` under "Claude for Daily Work."

---

## Quick Reference: Which File Does What?

| File | Role | When to use |
|---|---|---|
| `CLAUDE.md` (root) | Shared company context | Start of every session |
| `CLAUDE.md` (persona) | Your role identity and rules | Start of every session |
| `memory.md` | Project knowledge and history | On demand — load for complex sessions |
| `.claude/skills/<name>/SKILL.md` | Slash commands for role-specific tasks | Auto-registered — type `/skill-name` |
| `prompt_library.md` | Ready-made prompts (human reference) | Copy prompts into your session |
| `reference_card.md` | Prompt writing quick reference (human reference) | Keep open alongside session |
| `safety_checklist.md` | Review output before using | After every Claude output |
| `eval_checklist.md` | Score output quality | When deciding to use or iterate |
| `.mcp.json` | Tool connections | Auto-loaded at startup |
| `.claude/agents/<name>.md` | Autonomous multi-step tasks | When delegating a batch task |
| `.claude/settings.json` | Project-wide permissions and hooks | Auto-loaded at startup |
| `personas/<role>/settings.json` | Role-specific permissions and hooks | Auto-loaded at startup |
| `team_playbook.md` | Cross-team workflows | Facilitator and capstone sessions |
