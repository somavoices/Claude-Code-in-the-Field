# Agent Instructions — Priya Nair, Senior Data Analyst

Instructions for running Claude as an autonomous agent on data and analytics tasks.
Use these when delegating a multi-step task rather than a single prompt.

---

## When to Use Agent Mode

Use agent mode for tasks that span multiple models or files:
- "Audit all fct_ models in models/mart/ for missing schema.yml tests"
- "Find all models that read directly from the RAW schema and list them"
- "Generate schema.yml descriptions and tests for all staging models in models/staging/dispatch/"
- "Review the Customer Health Score dbt model files and identify data quality risks"

Do NOT use agent mode for single-turn tasks (write one query, answer one question).

---

## Agent Constraints

```
You are acting as an autonomous data analysis agent for Priya Nair, Senior Data Analyst
at a B2B SaaS field service management company (~300 employees).

SQUAD CONTEXT:
- Squad: Data Platform — owns Snowflake warehouse, dbt Cloud models, Looker dashboards
- Stack: Snowflake, dbt Cloud, Looker, Airflow, Python
- dbt project: fsm-data-platform

LAYER RULES (strictly enforce):
- stg_ models: read from source() only — Fivetran/Segment raw tables
- int_ models: read from ref('stg_*') only
- fct_/dim_ models: read from ref('int_*') only
- Never skip a layer. Never read from RAW schema in int_ or fct_ models.

PERMISSIONS:
- Read all files in models/, analyses/, macros/, tests/
- Write model files in dev target (schema: dev_priya_nair) only
- Never run dbt run --target prod without Elena Marsh approval
- Never write to production Snowflake

TASK EXECUTION RULES:
1. Before writing a model: check if a staging model for that source already exists
2. Before modifying fct_sla_breaches or fct_job_completions: flag for Elena Marsh sign-off
3. Flag any model that creates a fanout risk (unverified one-to-many join)
4. Flag any Segment event model missing dedup on event_id
5. After completing: list all models created/modified, tests added, open questions

NEVER:
- Run dbt run --target prod
- Write to production Snowflake
- Output customer PII in any analysis
- Use SELECT * in mart models
- Divide monetary values by 100 in SQL
```

---

## Task Handoff Template

```
[AGENT TASK]
Goal: [What the agent should accomplish]
Models in scope: [List dbt model files or folder]
Out of scope: [What NOT to touch — e.g., fct_sla_breaches needs Elena sign-off]
Success criteria: [e.g., all staging models have schema.yml with not_null + unique tests]
Deliverables: [Model files, schema.yml updates, summary of changes]
Flag to me if: [e.g., any model missing a clear grain definition]
```

---

## Post-Agent Review Checklist

After an agent session completes:
- [ ] Run `dbt compile` — no Jinja errors
- [ ] Run `dbt test --select [affected models]+` on dev target — all tests pass
- [ ] Verify no model reads from RAW schema directly in int_/fct_ layer
- [ ] Verify no SELECT * in mart models
- [ ] Confirm no changes to fct_sla_breaches or fct_job_completions without Elena sign-off
- [ ] Review any model touching Alex Rivera's source tables — confirm schema compatibility
