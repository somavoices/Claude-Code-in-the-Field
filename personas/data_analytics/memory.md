# memory.md — Priya Nair, Senior Data Analyst

## Project Context

**Customer Health Score (FSM-1402):**
- Three input signals: product usage (Segment `job_viewed`, `job_started`, `job_completed` events),
  Zendesk open ticket volume (last 30 days), Salesforce `renewal_date` proximity
- Grain: one row per customer account per week
- Known data quality issue: Segment has ~2% duplicate events — dedup by `event_id` in staging
- Salesforce `renewal_date` field is unreliable for multi-year contracts — validate with CS team lead

**SLA Breach Analysis:**
- Source: `fct_sla_breaches` — joins `dispatch_jobs` (via Fivetran from product DB)
  with `dim_customer` and `dim_technician`
- SLA thresholds by customer tier: Enterprise = 4h, Business = 8h, Starter = 24h
- Known issue: jobs with status `cancelled` are incorrectly included in breach counts — fix pending
  (FSM-1389, assigned to me, low priority until Health Score ships)

## Domain Rules

- Job statuses: `pending → assigned → en_route → on_site → complete | cancelled`
- SLA clock starts at `job.scheduled_at`, stops at `job.completed_at` — nulls mean incomplete
- Technician region comes from `dim_technician.region` — not from the job record
- Fiscal year is Jan–Dec (same as calendar year) — use `dim_date.fiscal_year`
- Never mix `fct_` models from different grains in the same query without explicit join key

## People & Relationships

- **Elena Marsh (Head of Data):** My manager. Approves prod dbt runs and any change to core
  fct_ models. Prefers written summaries over Slack voice messages. Reviews work on Thursdays.
- **Sam Okafor (PM):** Primary dashboard consumer. Asks "so what does this mean for the business?"
  — always lead with the insight, then the numbers. Will escalate if a dashboard is wrong
  before telling me.
- **Alex Rivera (Dispatch Core):** Alex's team owns the source tables I depend on. He's reliable
  about heads-up on schema changes, but he's busy with Project Falcon — ping him early, not late.
- **Morgan Blake (Ops):** Uses SLA dashboard every Monday for post-mortem prep. Needs exact
  numbers, not rounded. Has flagged two dashboard errors in the past — take his reports seriously.

## Incidents & Lessons

- **Feb 28 — SLA dashboard outage:** Fivetran sync failed silently; dashboard showed stale data
  for 3 days before Morgan noticed. Now: added a `dbt source freshness` check to Airflow DAG
  that pages me if `dispatch_jobs` source is >6 hours stale.
- **Jan 15 — Health Score prototype:** Used mean aggregation for Zendesk ticket volume —
  outlier customers (>50 tickets) skewed the score. Switched to median. Always check
  distribution before choosing aggregation method.

## Systems

- dbt Cloud project: `fsm-data-platform` — dev target is personal schema, prod is `ANALYTICS`
- Snowflake: `FSM_PROD` database, schemas: `RAW` (Fivetran), `STAGING`, `INTERMEDIATE`, `MART`
- Looker: `looker.company.internal` — LookML repo: `org/fsm-looker`
- Airflow: DAGs in `org/fsm-airflow` — dbt run DAG: `dbt_daily_run` (triggers 3am UTC)
- Jira board: FSM — data tickets tagged `data-platform`
