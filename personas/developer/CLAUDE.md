# CLAUDE.md — Full Stack Engineer (Alex Rivera)

## Setup

This file lives at `personas/developer/CLAUDE.md`. The shared company context lives at the project root `CLAUDE.md`.

Claude Code loads `CLAUDE.md` files hierarchically — it reads the root file **and** any `CLAUDE.md` in subdirectories automatically. No manual loading needed. Just start Claude Code from inside the `personas/developer/` folder, or from the project root with your working path set to this folder.

```
claude_code_persona/          ← project root
├── CLAUDE.md                 ← shared company context (auto-loaded)
└── personas/
    └── developer/
        ├── CLAUDE.md         ← this file (auto-loaded)
        └── ...
```

Both files are loaded automatically. No conflict, no renaming required.

---

## Who I Am

I am **Alex Rivera**, Full Stack Engineer on the **Dispatch Core** squad at a mid-size B2B SaaS
company (~300 employees) building a field service management platform. Our squad owns the
scheduling engine, job assignment logic, and real-time technician tracking.

Stack: Node.js/TypeScript (backend), React/TypeScript (frontend), PostgreSQL, Redis, AWS,
Terraform. CI/CD via GitHub Actions. Tests: Jest (unit/integration), Cypress (E2E).
Code quality gate: SonarQube on every PR — 80% coverage minimum, zero critical issues to merge.

**My squad colleagues:** Jordan Lee (QA), Sam Okafor (PM).
**Cross-team:** Priya Nair (Data Platform — consumes our events via Snowflake),
Chris Wang (BA — Integrations squad, owns API v2 requirements),
Morgan Blake (Ops/Support — on-call for our services in production).

---

## Active Work (Q2)

**Project Falcon — Real-Time Dispatch Redesign (FSM-1201):**
Rebuilding the scheduling engine to support dynamic re-routing of technicians mid-shift.
Replacing a polling-based architecture with an event-driven model (RabbitMQ).
Target ship: May 30. Jordan owns test plan. Sam owns acceptance criteria.

**Known risk:** Legacy `job_assignment` module has zero unit tests and 3 known race conditions
under concurrent dispatch load. Any changes to this module require Jordan's sign-off before merge.

**Open decisions:**
- Redis pub/sub vs. RabbitMQ for real-time dispatch events (Staff Engineer review due April 18)
- Optimistic locking vs. DB transactions on `dispatch_jobs` table (not yet decided)

---

## How Claude Should Behave

**Tone:** Peer-level, technical, concise. Skip preamble. I know what I'm asking.

**Code conventions:**
- TypeScript strict mode — no `any`, no implicit returns
- Async/await over raw promises
- Named exports over default exports
- Co-located tests: `*.test.ts` next to source file
- No `console.log` — use `import { logger } from '@/lib/logger'`
- All monetary values in cents (integer), never floats

**SQL conventions:**
- CTEs over subqueries
- Explicit column lists — no `SELECT *`
- Migrations in `/db/migrations/`, named `YYYYMMDD_description.sql`
- Never write DROP or TRUNCATE without explicit instruction

**When reviewing code, always flag:**
- Direct `db.query()` calls that bypass the query builder
- Missing error handling on async functions
- Hardcoded environment values or secrets
- N+1 query patterns

---

## What Claude Must Never Do

- Push to `main` or `release/*` branches
- Suggest disabling SonarQube quality gates
- Write DROP/TRUNCATE/DELETE without a WHERE clause
- Commit `.env` files or secrets
- Make schema changes without a Jira `db-migration` tagged ticket
