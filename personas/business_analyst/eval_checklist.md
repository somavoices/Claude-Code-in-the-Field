# Eval Checklist — Chris Wang, Business Analyst

Use after Claude produces output. Score each section and decide: use as-is / revise / reject.

---

## Functional Requirements Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Uses "shall" / "should" — not "must", "will", "needs to" | | | |
| Every requirement is verifiable (a tester can write a test from it) | | | |
| Every requirement traces to a source (Jira/customer/product decision) | | | |
| Error handling documented: 401, 429, 400, 500 minimum | | | |
| All timestamps specified as ISO 8601 UTC | | | |
| Rate limits specified per customer tier | | | |
| Authentication requirement stated (Bearer token) | | | |
| No implementation detail — behaviour only | | | |
| No customer names or PII | | | |
| Requirement IDs are sequential (REQ-API2-NNN) | | | |

**Score threshold:** 9/10. Any PII or product decision made by Claude = reject immediately.

---

## Process Flow Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| All actors identified | | | |
| Happy path numbered and sequential | | | |
| All exception paths labelled and resolved (not "error occurs") | | | |
| Every decision point has an explicit Yes/No branch | | | |
| End states are defined for every path | | | |
| System ownership is clear for each step | | | |

**Score threshold:** 5/6.

---

## Data Mapping Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Transformation rule is explicit for every field | | | |
| PII fields are flagged, not populated with real data | | | |
| New v2 fields distinguished from v1 fields | | | |
| Type changes are explicitly noted | | | |
| Ambiguous mappings are flagged as open questions | | | |

**Score threshold:** 4/5.

---

## Iteration Log

| Date | Task | Prompt quality (1–5) | What worked | What to change next time |
|---|---|---|---|---|
| | | | | |
