# Eval Checklist — Priya Nair, Senior Data Analyst

Use after Claude produces output. Score each section and decide: use as-is / revise / reject.

---

## dbt Model Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Correct layer: stg_ reads from source(), int_/fct_ reads from ref() | | | |
| No SELECT * in mart models | | | |
| _fivetran_deleted = false filtered in staging | | | |
| Segment events deduped on event_id in staging | | | |
| All timestamps cast to timestamp_ntz | | | |
| Monetary values stay as integers (cents) — not divided by 100 | | | |
| CTEs named descriptively — not "cte1", "final_data" | | | |
| schema.yml included with not_null + unique tests on PK | | | |
| Grain is correct for the layer — no fan-out from unhandled one-to-many | | | |

**Score threshold:** 8/9. Monetary float or wrong layer source = reject immediately.

---

## SQL Query Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Uses CTEs over subqueries | | | |
| Explicit column list — no SELECT * | | | |
| Aggregation is at the correct grain | | | |
| Window functions have correct PARTITION BY and ORDER BY | | | |
| Would benefit from EXPLAIN ANALYZE on large tables? (flag if yes) | | | |

**Score threshold:** 4/5.

---

## Stakeholder Summary Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Leads with business insight, not metric name | | | |
| States the number precisely | | | |
| Explains the method in one sentence — no SQL/dbt jargon | | | |
| Ends with a recommended action or question | | | |
| Contains no customer PII | | | |

**Score threshold:** 5/5. Any PII = reject immediately.

---

## Iteration Log

| Date | Task | Prompt quality (1–5) | What worked | What to change next time |
|---|---|---|---|---|
| | | | | |
