# Prompt Library — Priya Nair, Senior Data Analyst

## How to Use This File

**Do not load this file into Claude.** This is a reference for you, not context for Claude.

1. Find the prompt that matches your task
2. Copy it into your Claude Code session
3. Replace the `[PLACEHOLDER]` values with your actual input
4. Run it — Claude already has your context from `CLAUDE.md` and `memory.md`

Adapt prompts over time. If a prompt consistently needs the same tweak, edit this file.

---

## 1. Write a dbt Model

```
Write a dbt model for the Data Platform squad at a B2B SaaS field service management company.

Warehouse: Snowflake. Layers: stg_ (staging), int_ (intermediate), fct_/dim_ (mart).
Source tables come from Fivetran (product PostgreSQL) and Segment (events).

Requirements:
- Follow the stg_ → int_ → fct_/dim_ layer pattern — never skip a layer
- Use CTEs with descriptive names
- Explicit column lists — no SELECT *
- Filter _fivetran_deleted = false in staging
- Dedup Segment events on event_id in staging
- Include schema.yml with description, not_null + unique tests on PK
- All timestamps cast to timestamp_ntz (UTC)
- Monetary values stay in cents — no division by 100

Model I need: [DESCRIBE WHAT THE MODEL SHOULD DO AND ITS GRAIN]
Source tables available: [LIST SOURCES]
```

---

## 2. Investigate a Metric Discrepancy

```
Help me investigate a discrepancy in SLA metrics for the field service management platform.

Context:
- SLA breach definition: job.completed_at - job.scheduled_at > SLA threshold by customer tier
  (Enterprise=4h, Business=8h, Starter=24h)
- SLA clock: starts at scheduled_at, stops at completed_at
- Known issue: cancelled jobs are incorrectly included in breach counts (FSM-1389, not yet fixed)
- Timestamps are UTC; all stored as timestamp_ntz in Snowflake

Discrepancy: [DESCRIBE WHAT YOU EXPECTED VS. WHAT YOU SEE]
Affected model/dashboard: [NAME]
Date range: [RANGE]

Walk me through: (1) likely causes ranked by probability, (2) SQL to confirm each,
(3) whether this needs Elena Marsh sign-off to fix.
```

---

## 3. Stakeholder Summary

```
Translate this data finding into a plain-language summary for a non-technical stakeholder.

Audience: [Sam Okafor (PM) / CS team lead / executive]
Context: B2B SaaS field service management platform, ~300 employees.

Rules:
- Lead with the business insight, not the metric name
- State the number precisely — no rounding unless >1M
- One sentence explaining the method — no SQL, no dbt jargon
- End with one recommended action or question to investigate further
- Do not include raw customer names or PII

Finding: [PASTE YOUR ANALYSIS OR NUMBERS HERE]
```

---

## 4. dbt Model Review

```
Review this dbt model for correctness, performance, and conventions used by the
Data Platform squad at a B2B SaaS field service management company.

Check for:
- Correct layer usage (stg_ reads from source(), int_/fct_ reads from ref())
- SELECT * usage (not allowed in mart models)
- Missing _fivetran_deleted filter in staging
- Segment event dedup on event_id
- Fanout risk in joins (verify grain before aggregating)
- Missing schema.yml tests (not_null, unique on PK required)
- Incremental model: missing is_incremental() guard or unique_key
- Monetary values divided by 100 in SQL (should stay as cents)

Model:
[PASTE MODEL SQL HERE]

Model layer: [stg_ / int_ / fct_ / dim_]
```

---

## 5. Query Optimisation

```
Optimise this Snowflake SQL query. It is used in a dbt model that runs daily on the
fsm-data-platform project. Current runtime: [X seconds/minutes].

Table sizes (approximate):
- dispatch_jobs: ~50M rows
- technician_locations: ~200M rows
- dim_customer: ~5K rows

Check for:
- Full table scans that could use clustering keys
- Unnecessary cross joins or missing join conditions
- Window functions that could be replaced with aggregations
- CTEs that are referenced only once (can be inlined)
- Cartesian products from missing join predicates

Query:
[PASTE QUERY HERE]
```

---

## 6. Airflow DAG Review

```
Review this Airflow DAG used in the fsm-data-platform pipeline.

Context:
- DAG triggers dbt daily at 3am UTC
- Source freshness check on dispatch_jobs must complete before dbt run
- If source freshness fails, page Priya Nair via PagerDuty (do not silently skip)
- dbt run must complete before Looker PDT refresh

Check for:
- Missing depends_on_past or catchup settings
- Missing failure alerting (source freshness failures must page)
- Task ordering: freshness check → dbt run → Looker PDT
- Hardcoded credentials or connection strings
- Missing retries on transient Snowflake connection failures

DAG:
[PASTE DAG CODE HERE]
```
