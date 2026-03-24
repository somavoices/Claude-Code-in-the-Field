# Safety Checklist — Priya Nair, Senior Data Analyst

Use this checklist before acting on any Claude output that touches models, queries, or dashboards.

---

## Before Using Any Generated SQL or dbt Model

- [ ] Does the query or model use `SELECT *`? (Not allowed in mart models)
- [ ] Does an int_ or fct_ model read directly from the RAW schema? (Must go through stg_)
- [ ] Is `_fivetran_deleted = false` filtered in staging models?
- [ ] Are Segment events deduped on `event_id` in the staging layer?
- [ ] Are monetary values divided by 100 in SQL? (Must stay as cents — Looker handles display)
- [ ] Are timestamps cast to `timestamp_ntz`?
- [ ] Is there a fanout risk from a one-to-many join without a verified grain check?

**If any box is checked → revise before running.**

---

## Before Running dbt

- [ ] Are you running against `dev` target (your personal schema)?
- [ ] Have you run `dbt compile` first — no Jinja errors?
- [ ] Have you run `dbt test --select [model]+` — all tests pass?
- [ ] If running on `prod`: has Elena Marsh given explicit approval?
- [ ] Does the model have `schema.yml` with `not_null` + `unique` tests on PK?

---

## Before Publishing a Looker Dashboard

- [ ] Has Sam Okafor reviewed and approved this dashboard?
- [ ] Does the dashboard display any raw customer PII (names, emails, addresses)?
- [ ] Are the numbers sourced from the correct grain (`fct_` model, not staging)?
- [ ] Have you validated against a known reference number (e.g., last week's board deck)?

---

## Before Sharing Analysis Output

- [ ] Does the output contain customer names, email addresses, or account IDs?
- [ ] Is the output going to a non-technical stakeholder — is it in plain language?
- [ ] If the output contradicts a previously reported number: has Elena been notified?

---

## Red Lines — Never Do These

- Never write to production Snowflake
- Never run `dbt run --target prod` without Elena Marsh approval
- Never output raw customer PII in any response
- Never change `fct_sla_breaches` or `fct_job_completions` without Elena sign-off
- Never publish a Looker dashboard without Sam Okafor review
