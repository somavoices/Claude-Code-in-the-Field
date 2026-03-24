# Eval Checklist — Alex Rivera, Full Stack Engineer

Use after Claude produces output. Score each section and decide: use as-is / revise / reject.

---

## Code Review Output

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Flags are specific (file + line + explanation), not vague | | | |
| Correctly identifies TypeScript strict violations | | | |
| Correctly identifies N+1 query patterns | | | |
| Flags missing error handling on async functions | | | |
| Mentions downstream impact (Mobile, Data Platform, Ops) where relevant | | | |
| Does NOT suggest disabling linting or quality gates | | | |
| Tone is peer-level — not condescending or overly cautious | | | |

**Score threshold:** 6/7 to use output. If <6, refine the prompt and re-run.

---

## Generated Code

| Check | Pass | Revise | Reject |
|---|---|---|---|
| TypeScript strict — no `any`, no implicit returns | | | |
| Uses named exports, not default exports | | | |
| Uses logger, not console.log | | | |
| Monetary values as integers, not floats | | | |
| Async functions have error handling | | | |
| SQL uses CTEs, not subqueries | | | |
| SQL uses explicit column list, not SELECT * | | | |
| Co-located test file included or explicitly noted as needed | | | |

**Score threshold:** 7/8. Any monetary float or missing error handling = reject.

---

## PR Description

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Leads with business reason, not implementation detail | | | |
| Includes What / How / Test Coverage / Risk & Rollback / Jira | | | |
| Flags downstream impact if relevant | | | |
| Risk & Rollback section is present and non-trivial | | | |
| Jira ticket is referenced | | | |

**Score threshold:** 5/5.

---

## Iteration Log

Track which prompts worked well and which needed revision.

| Date | Task | Prompt quality (1–5) | What worked | What to change next time |
|---|---|---|---|---|
| | | | | |
