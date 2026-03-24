# Safety Checklist — Alex Rivera, Full Stack Engineer

Use this checklist before acting on any Claude output that touches code, data, or infrastructure.

---

## Before Using Any Generated Code

- [ ] Does the code contain hardcoded secrets, API keys, or credentials?
- [ ] Does any SQL contain DELETE, DROP, or TRUNCATE without a WHERE clause?
- [ ] Does any migration lack a rollback script?
- [ ] Does the code bypass the query builder with raw `db.query()` calls?
- [ ] Does the code use `any` type or suppress TypeScript strict errors?
- [ ] Does the code use `console.log` instead of the logger?
- [ ] Are monetary values stored as floats (must be integers/cents)?
- [ ] Does the code introduce a new external dependency not approved by Ravi Sharma?

**If any box is checked → revise before committing.**

---

## Before Running Generated SQL

- [ ] Have you run `EXPLAIN ANALYZE` on queries touching `dispatch_jobs` or `technician_locations`?
- [ ] Is this being run on staging, not production?
- [ ] Does the query affect >10,000 rows? If yes — get a second engineer review.
- [ ] Is there a rollback plan if this migration goes wrong?

---

## Before Opening a PR

- [ ] Is the target branch `feature/*` or `fix/*` — not `main` or `release/*`?
- [ ] Is there a linked Jira ticket (FSM-XXXX)?
- [ ] Does the PR description include downstream impact (Mobile squad, Data Platform, Ops)?
- [ ] Has SonarQube passed (80% coverage, zero critical issues)?
- [ ] Has Jordan Lee been tagged if the change touches `job_assignment` or `status-machine`?
- [ ] Has Ravi Sharma been tagged if this is an architectural change?

---

## Before Sharing Claude Output With Others

- [ ] Does the output contain internal architecture details that shouldn't be external?
- [ ] Does the output contain customer PII?
- [ ] Has the output been reviewed — not just copy-pasted?

---

## Red Lines — Never Do These

- Never push to `main` or `release/*` directly
- Never disable or bypass SonarQube quality gates
- Never commit `.env` files or secrets
- Never run DROP/TRUNCATE/DELETE without a WHERE clause
- Never make schema changes without a `db-migration` Jira ticket
