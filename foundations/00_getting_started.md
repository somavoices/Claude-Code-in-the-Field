# 00 — Getting Started
## What Is Claude Code and Why Should You Use It?

---

## Before You Read Anything Else

If you have never used Claude or any AI assistant before, start here.
This document explains what Claude Code is, what it can do for your specific role,
and what it cannot do. Everything else in this training kit builds on these foundations.

**Estimated reading time: 10 minutes.**

---

## What Is Claude?

Claude is an AI assistant made by Anthropic. You interact with it by typing messages
in plain English (or any language), and it responds — writing text, code, analyses,
summaries, plans, and more.

Think of it as a highly capable colleague who:
- Has read an enormous amount of documentation, code, writing, and research
- Can draft, review, explain, and improve almost any type of work
- Responds instantly and never gets tired
- Has no memory of previous conversations unless you give it context

**The last point is critical.** Claude does not remember you from one session to the
next. Every conversation starts fresh — unless you provide background information
at the start. This is exactly what the CLAUDE.md and memory.md files in this kit do:
they give Claude the context it needs to behave like someone who knows you and your work.

---

## What Is Claude Code?

Claude Code is Anthropic's command-line tool that brings Claude directly into your
development and work environment. Instead of switching to a browser to chat with
Claude, you run it from your terminal alongside your actual work — code files,
documents, configurations, databases, and tools.

**The difference between Claude (web chat) and Claude Code:**

| Claude Web Chat | Claude Code |
|---|---|
| You type in a browser | You run in your terminal |
| Claude sees only what you paste | Claude can read your actual files |
| No connection to your tools | Can connect to GitHub, databases, filesystems |
| General-purpose | Integrated into your workflow |
| Manual copy-paste | Works with your code and documents directly |

For most of the roles in this training, Claude Code is significantly more powerful
than web chat because it can read your actual codebase, query your actual database
(read-only), and work with your real documents — not just examples you paste in.

---

## What Can Claude Do For Your Role?

### Developer
- Review pull request diffs and flag issues by file and line number
- Generate unit tests for functions you write
- Debug errors using your actual logs and code
- Write PR descriptions that explain the business reason behind a change
- Review database migrations for missing indexes and rollback scripts

### Data & Analytics
- Write dbt models following your team's layer conventions
- Translate complex metric discrepancies into plain-language explanations
- Review SQL queries for performance problems before they hit production
- Summarise data findings for non-technical stakeholders

### QA & Testing
- Generate Cypress E2E tests from acceptance criteria
- Write manual test plans in a format ready for TestRail
- Triage flaky tests by analysing patterns in the failure
- Write bug reports that are reproducible, factual, and appropriately prioritised

### Product Manager
- Draft PRDs with structured problem statements and Given/When/Then ACs
- Synthesise customer discovery interview notes into clear insights
- Write stakeholder status updates at the right level for each audience
- Draft release notes in plain language for non-technical customers

### Business Analyst
- Write functional requirements using correct shall/should language
- Document process flows with all exception paths covered
- Create data mapping tables with explicit transformation rules
- Produce gap analyses between current and future system behaviour

### Ops & Support
- Write operational runbooks with concrete, executable steps
- Draft blameless post-mortems from incident timelines
- Compose Zendesk responses that acknowledge impact and avoid speculation
- Build Datadog monitor configurations with correct escalation paths

---

## What Claude Cannot Do

Understanding Claude's limitations is as important as understanding its capabilities.

**Claude can be wrong.** It does not look things up in real time. Its knowledge
has a training cutoff date. It can confidently produce incorrect code, incorrect
SQL, or incorrect requirements. Every output needs to be reviewed by a human
before it is used.

**Claude does not know your systems.** Without context files loaded, it has no idea
what your codebase looks like, what your team's conventions are, or who your
colleagues are. Generic Claude produces generic output. Context-loaded Claude
produces role-specific, team-specific output. This is why the CLAUDE.md, memory.md,
and SKILL.md files exist.

**Claude cannot make decisions for you.** It can draft a PRD, but it cannot decide
what to build. It can suggest a fix, but it cannot decide whether the fix is right
for your system. It is a thinking partner, not a decision-maker.

**Claude cannot take irreversible actions safely on its own.** This is why the
settings.json permissions and hooks exist — to ensure Claude never pushes to
production, never sends a customer email, never runs a destructive database
command without human review.

---

## The Mental Model: Claude as a New Colleague

The most useful way to think about Claude Code is as a **highly capable new
colleague on your first day**.

They are smart, well-read, and eager to help. But they do not know:
- Your company's name or what product you build
- Your team's conventions and standards
- The incidents that happened last quarter
- Who the key stakeholders are
- Which parts of the codebase are dangerous to touch

You would not let a new colleague push code to production on their first day.
You would brief them on context, give them defined tasks, and review their work
before it goes anywhere important.

That is exactly how to use Claude Code. Brief it with context (CLAUDE.md, memory.md),
give it defined tasks (prompts from the prompt library), and review its output
(safety checklist, eval checklist) before acting on it.

---

## How This Training Kit Is Structured

The training kit is organised in phases. Each phase builds on the previous one.

| Phase | What You Learn | Files |
|---|---|---|
| **Foundations** (you are here) | What Claude Code is, how to set it up, core concepts | `foundations/` folder |
| **Phase 1** | How to write prompts, safety basics | `prompt_library.md`, `reference_card.md`, `safety_checklist.md` |
| **Phase 2** | Daily use — context, memory, skills | `CLAUDE.md`, `memory.md`, `SKILL.md` |
| **Phase 3** | Integration — tools, evaluation | `mcp_config.json`, `eval_checklist.md` |
| **Phase 4** | Automation — agents, hooks | `agent_instructions.md`, `settings.json` hooks |

**Read the foundations documents first.** The persona-specific files (CLAUDE.md,
memory.md, etc.) will not make sense without the foundations.

---

## Your Next Step

Go to [01_installation_setup.md](01_installation_setup.md) to install Claude Code
and verify it is working on your machine.
