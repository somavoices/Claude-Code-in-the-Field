# Prompt Library — Alex Rivera, Full Stack Engineer

---

## 1. Code Review

```
Review this pull request diff for the Dispatch Core squad at a B2B SaaS field service management company.

Stack: Node.js/TypeScript, PostgreSQL, React/TypeScript. Tests: Jest, Cypress.
Quality gate: SonarQube (80% coverage minimum, zero critical issues).

Check for:
- TypeScript strict violations (any, implicit returns, missing types)
- Missing or insufficient error handling on async functions
- N+1 query patterns or missing DB indexes
- Direct db.query() calls bypassing the query builder
- Hardcoded environment values or secrets
- Monetary values stored as floats (must be integers/cents)
- Race conditions in concurrent dispatch operations
- Missing unit tests for new logic

For each issue: state the file/line, explain the risk, suggest the fix.
Flag anything that affects downstream consumers (Mobile squad, Data Platform, Ops).

[PASTE DIFF HERE]
```

---

## 2. PR Description

```
Write a GitHub PR description for the following change on the fsm-platform monorepo.

Use this format:
## What
## How
## Test Coverage (with checklist)
## Risk & Rollback
## Jira

Rules:
- Lead with the business reason, not the implementation detail
- If the change touches dispatch_events schema, call out impact on Priya Nair (Data Platform)
- If the change touches API contracts, call out impact on Chris Wang (BA) and Mobile squad
- If error codes or log formats change, note that Morgan Blake (Ops) must be notified
- Include a rollback note if this is a DB migration

Change description: [DESCRIBE YOUR CHANGE]
Jira ticket: FSM-[XXXX]
```

---

## 3. Debug Assistance

```
Help me debug this issue in the Dispatch Core service.

Context:
- Service: Node.js/TypeScript, PostgreSQL, Redis
- Environment: [staging / production]
- Symptom: [DESCRIBE WHAT IS HAPPENING]
- When it started: [DATE/TIME or deploy]
- Frequency: [always / intermittent — approx X% of requests]
- Relevant logs: [PASTE LOGS]
- Relevant code: [PASTE CODE SNIPPET]

Known constraints:
- job_assignment module has 3 known race conditions under concurrent load
- Technician location updates fire every 30 seconds — timing-sensitive
- All timestamps stored in UTC

Walk me through: (1) likely root causes ranked by probability, (2) how to confirm each,
(3) the safest fix that avoids touching the job_assignment module if possible.
```

---

## 4. Unit Test Generation

```
Write Jest unit tests for the following TypeScript function used in the Dispatch Core service.

Requirements:
- Co-located test file: [filename].test.ts
- Cover: happy path, all error paths, edge cases (empty input, null, boundary values)
- Mock external dependencies (DB, Redis, logger) — do not make real calls
- Use descriptive test names: "should [expected behaviour] when [condition]"
- Assert on specific error messages, not just that an error was thrown
- Target: 100% branch coverage for this function

Function to test:
[PASTE FUNCTION HERE]
```

---

## 5. Commit Message

```
Write a git commit message for the following change.

Format:
<type>(<scope>): <short summary under 72 chars>

[Body: what changed and why — not how. Reference the root cause or business reason.]

[Footer: Jira ticket reference]

Types: feat, fix, refactor, test, chore, docs
Scope: use the module name (e.g., dispatch, job-assignment, sla, api, db)

Rules:
- Summary line must be under 72 characters
- Body must explain WHY, not just what files changed
- If this fixes an incident, reference it (e.g., "Fixes race condition from Feb 14 incident")
- If downstream consumers are affected, note it in the body

Change description: [DESCRIBE YOUR CHANGE]
Jira: FSM-[XXXX]
```

---

## 6. DB Migration Review

```
Review this PostgreSQL migration for the fsm-platform before I raise it for Staff Engineer sign-off.

Check for:
- Missing indexes on foreign keys or columns used in WHERE clauses
- Missing rollback script (every migration needs one)
- Lock risks on large tables (dispatch_jobs, technician_locations have millions of rows)
- Constraint naming conventions (fk_, idx_, uq_ prefixes)
- Whether the migration is backwards-compatible with the currently deployed app version
- Any impact on Priya Nair's Data Platform models (she reads from dispatch_jobs, technician_locations)

Migration SQL:
[PASTE MIGRATION HERE]

Table sizes (approximate): [PROVIDE IF KNOWN]
```
