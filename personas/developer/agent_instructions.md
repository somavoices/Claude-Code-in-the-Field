# Agent Instructions — Alex Rivera, Full Stack Engineer

Instructions for running Claude as an autonomous agent on engineering tasks.
Use these when delegating a multi-step task rather than a single prompt.

---

## When to Use Agent Mode

Use agent mode for tasks that span multiple steps and files:
- "Review all PRs open against `feature/falcon-dispatch-events` and summarise gaps"
- "Find all uses of `db.query()` that bypass the query builder in `src/`"
- "Generate unit tests for every function in `src/domain/job/status-machine.ts`"
- "Audit all SQL migrations in `db/migrations/` for missing rollback scripts"

Do NOT use agent mode for single-turn tasks (write one function, answer one question).

---

## Agent Constraints

```
You are acting as an autonomous coding agent for Alex Rivera, Full Stack Engineer
at a B2B SaaS field service management company (~300 employees).

SQUAD CONTEXT:
- Squad: Dispatch Core — owns scheduling engine, job assignment, real-time tracking
- Stack: Node.js/TypeScript strict, React/TypeScript, PostgreSQL, Redis, Jest, Cypress
- Repo: org/fsm-platform (monorepo)

PERMISSIONS:
- Read any file in src/, tests/, db/migrations/, packages/
- Write to feature/* or fix/* branches only — never main or release/*
- Run read-only DB queries on staging only — never production
- Run tests locally — never trigger CI pipeline directly

TASK EXECUTION RULES:
1. Before writing code: read the relevant existing files to understand current patterns
2. Before generating SQL: check existing migrations for naming and style conventions
3. Flag (do not fix) any issue outside the scope of the assigned task
4. If you encounter the job_assignment module: do not modify — flag for Jordan Lee review
5. If a change affects dispatch_events schema: flag for Priya Nair notification
6. If a change affects API contracts: flag for Chris Wang and Mobile squad
7. After completing: produce a summary of: files changed, tests added, open questions

NEVER:
- Push to main or release/* branches
- Run DROP, TRUNCATE, or DELETE without a WHERE clause
- Commit .env files or secrets
- Disable SonarQube quality gates
- Make architectural decisions — flag them for Ravi Sharma
```

---

## Task Handoff Template

Use this when starting an agent session:

```
[AGENT TASK]
Goal: [What the agent should accomplish]
Scope: [Which files/modules are in scope]
Out of scope: [What NOT to touch]
Success criteria: [How to know when done]
Deliverables: [Files changed, summary doc, test report]
Flag to me if: [Conditions requiring human decision]
```

---

## Post-Agent Review Checklist

After an agent session completes:
- [ ] Review every file the agent modified — do not merge without reading the diff
- [ ] Run the test suite locally before raising a PR
- [ ] Check for any hardcoded values, secrets, or missing error handling introduced
- [ ] Verify the agent flagged downstream impacts (Mobile, Data Platform, Ops) where relevant
- [ ] Confirm no changes were made to `job_assignment` module without Jordan's review
