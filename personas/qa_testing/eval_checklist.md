# Eval Checklist — Jordan Lee, QA Engineer

Use after Claude produces output. Score each section and decide: use as-is / revise / reject.

---

## Cypress Test Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Uses data-testid selectors only (no CSS class or text selectors) | | | |
| Uses cy.intercept + cy.wait('@alias') — no cy.wait([number]) | | | |
| Each test creates its own data in beforeEach | | | |
| Test accounts use qa-test-*@company.internal format | | | |
| Happy path covered | | | |
| At least one error/failure path covered | | | |
| Test names follow "should [outcome] when [condition]" format | | | |
| No real customer data or PII in fixtures | | | |

**Score threshold:** 7/8. Any real PII or cy.wait([number]) = reject immediately.

---

## Manual Test Plan Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Happy path covered | | | |
| Error paths covered (invalid input, server error, network failure) | | | |
| Permission boundary covered (dispatcher vs. technician vs. admin) | | | |
| Empty state covered | | | |
| Severity assigned per test case (P1–P4) | | | |
| Each step has an explicit expected result | | | |

**Score threshold:** 5/6.

---

## Bug Report Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Steps are reproducible from scratch by any engineer | | | |
| Expected vs. actual clearly stated | | | |
| Severity is correctly assigned | | | |
| Environment and browser/version specified | | | |
| Tone is factual and neutral — no blame | | | |
| Jira ticket referenced | | | |

**Score threshold:** 6/6. Any blame language = reject.

---

## Iteration Log

| Date | Task | Prompt quality (1–5) | What worked | What to change next time |
|---|---|---|---|---|
| | | | | |
