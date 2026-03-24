# SKILL.md — Writing dbt Models (Data Platform Squad)

## When to Use
When creating or modifying a dbt model in the `fsm-data-platform` project on Snowflake.
Applies to staging, intermediate, and mart layer models.

---

## Model Layer Rules

| Layer | Prefix | Source | Purpose |
|---|---|---|---|
| Staging | `stg_` | Raw Fivetran/Segment tables only | Rename columns, cast types, dedup — no business logic |
| Intermediate | `int_` | Staging models only | Joins, reshaping, derived fields |
| Mart | `fct_` / `dim_` | Intermediate models only | Business-grain aggregations, final metrics |

**Never skip a layer.** Never read from `RAW` schema in an `int_` or `fct_` model.

---

## Model Template

```sql
-- models/[layer]/[name].sql
{{
  config(
    materialized = 'table',   -- or 'incremental' for large event tables
    schema = '[staging|intermediate|mart]'
  )
}}

with source as (
    select * from {{ source('fivetran_fsm', 'dispatch_jobs') }}
    -- or: ref('stg_dispatch_jobs') for int_/fct_ models
),

renamed as (
    select
        job_id,
        customer_id,
        technician_id,
        status,
        scheduled_at::timestamp_ntz    as scheduled_at_utc,
        completed_at::timestamp_ntz    as completed_at_utc,
        _fivetran_synced               as loaded_at
    from source
    where _fivetran_deleted = false
),

final as (
    select * from renamed
)

select * from final
```

---

## schema.yml Requirements

Every model must have:
```yaml
- name: stg_dispatch_jobs
  description: "Cleaned staging model for dispatch jobs from the product DB."
  columns:
    - name: job_id
      tests:
        - not_null
        - unique
    - name: status
      tests:
        - not_null
        - accepted_values:
            values: ['pending','assigned','en_route','on_site','complete','cancelled']
```

---

## Rules & Anti-Patterns

- Always filter `_fivetran_deleted = false` in staging
- Dedup Segment events on `event_id` in `stg_segment_events`
- Use `dim_date` for all date spine joins — never `generate_series` or `dateadd` loops
- For incremental models: always include `is_incremental()` guard and unique key
- Never use `SELECT *` in mart models — column list must be explicit
- Monetary values stay in cents — do not divide by 100 in SQL (Looker handles display)
- Flag fanout risk: if joining a one-to-many, verify grain before aggregating

---

## Before Running in Prod

1. Run `dbt compile` — confirm no Jinja errors
2. Run `dbt run --target dev --select [model]+` — check row counts make sense
3. Run `dbt test --select [model]+` — all tests must pass
4. Get Elena Marsh approval before `dbt run --target prod`
