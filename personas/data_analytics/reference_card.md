# Reference Card — Priya Nair, Senior Data Analyst

## Prompt Engineering Best Practices

### The 4-Part Prompt Structure
```
[CONTEXT]   Your role + the data domain (SLA, Health Score, dispatch events)
[TASK]      What model/query/analysis you need — specific grain and output
[RULES]     dbt layer, naming conventions, no SELECT *, monetary in cents
[INPUT]     Source tables, sample data, or existing model to modify
```

### Make Prompts Specific to Your Stack
| Vague | Specific |
|---|---|
| "Write a dbt model" | "Write a fct_ mart model at job grain, joining stg_dispatch_jobs and dim_customer, filtering cancelled jobs" |
| "Explain this discrepancy" | "SLA breach count is 12% higher than last week with no volume change — here are the two queries" |
| "Summarise for Sam" | "Translate this metric finding into 2 sentences for Sam Okafor (PM, non-technical) — lead with business impact" |

### Context Shortcuts
- **Stack:** "Snowflake, dbt Cloud, Looker — stg_ → int_ → fct_/dim_ layer pattern"
- **Domain:** "Field service management — jobs, technicians, SLA by tier (Enterprise 4h, Business 8h, Starter 24h)"
- **Rules:** "CTEs, explicit columns, no SELECT *, timestamps as timestamp_ntz UTC, monetary in cents"

---

## Claude for Daily Analytics Work

| Task | Best Prompt Type | Key Rule |
|---|---|---|
| Write dbt model | Describe grain + source tables + business rule | Specify which layer (stg/int/fct) |
| Debug discrepancy | Paste two queries + expected vs. actual | Include date range and filters |
| Stakeholder summary | Paste numbers + audience name | State what action you want them to take |
| Query optimisation | Paste query + table sizes | Include current runtime |
| schema.yml tests | Paste model columns | Always include not_null + unique on PK |
| Airflow DAG review | Paste DAG + pipeline description | Note what must run before what |

---

## Iteration Patterns

- **Model returns wrong grain** → "The grain should be one row per [X] per [Y] — rewrite the final CTE"
- **Claude skips a layer** → "This reads from RAW directly — rewrite to go through stg_ first"
- **Summary too technical** → "Rewrite for a non-technical PM — no SQL terms, no model names"
- **Missing edge case** → "What happens when completed_at is NULL? Add handling."

---

## Safety Reminders

- Never paste customer names or PII into prompts — use anonymised or synthetic data
- Never run `dbt run --target prod` without Elena Marsh approval
- Review all generated SQL on dev target before promoting to prod
- If Claude suggests reading from RAW schema in an int_ model: reject — must go through stg_
