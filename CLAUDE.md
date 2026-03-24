# CLAUDE.md — Shared Company Context

## Company

A mid-size B2B SaaS company, ~300 employees, building a **field service management platform**.
The platform helps enterprise customers schedule, dispatch, and track field technicians —
think utilities, telecoms, and facilities management. Core product: web app + mobile app
for technicians in the field.

**Stage:** Series C, post-product-market fit, scaling GTM and engineering in parallel.
**Engineering org:** ~60 engineers across 8 squads.
**Fiscal year:** Jan–Dec. Current quarter: Q2.

---

## Shared Systems

| System | Purpose |
|---|---|
| GitHub | Source control — all repos |
| Jira | Project tracking — board: FSM |
| Confluence | Internal documentation |
| Slack | Team communication |
| Datadog | Monitoring, logs, APM |
| PagerDuty | On-call alerting |
| Salesforce | CRM — sales pipeline and customer accounts |
| Snowflake | Data warehouse |
| dbt Cloud | Data transformation |
| Looker | BI dashboards |
| Zendesk | Customer support tickets |
| TestRail | QA test case management |

---

## Product Squads (relevant to these personas)

| Squad | Mission |
|---|---|
| **Dispatch Core** | Scheduling engine, job assignment, real-time technician tracking |
| **Mobile** | Technician mobile app (iOS/Android) |
| **Integrations** | Customer-facing API, webhooks, third-party connectors |
| **Data Platform** | Data warehouse, analytics, reporting |
| **Platform & Reliability** | Infrastructure, CI/CD, on-call, incident response |

---

## The Six Personas — Colleagues at This Company

All six personas work at the same company, on related work streams.
They share the same Jira board (FSM), the same GitHub org, and the same Slack workspace.

| Persona | Name | Role | Squad |
|---|---|---|---|
| Developer | **Alex Rivera** | Full Stack Engineer | Dispatch Core |
| Data & Analytics | **Priya Nair** | Senior Data Analyst | Data Platform |
| QA & Testing | **Jordan Lee** | QA Engineer | Dispatch Core |
| Product Manager | **Sam Okafor** | Product Manager | Dispatch Core |
| Business Analyst | **Chris Wang** | Business Analyst | Integrations |
| Ops & Support | **Morgan Blake** | Senior Support Engineer | Platform & Reliability |

---

## Shared Active Projects (Q2)

**Project Falcon — Real-Time Dispatch Redesign:**
Rebuilding the scheduling engine to support dynamic re-routing of technicians mid-shift.
Cross-functional: Alex (engineering), Jordan (QA), Sam (PM), Chris (BA requirements).
Target ship: May 30.

**Customer Health Score Dashboard:**
Priya building a Snowflake/dbt model to power a Looker dashboard for the CS team.
Inputs: product usage, Zendesk ticket volume, contract renewal date.
Target: end of Q2.

**Enterprise API v2:**
Chris leading requirements for the new public API (REST → versioned REST + webhooks).
Involves Alex (implementation), Morgan (support runbooks for API errors).
Target: June 15.

---

## Shared Constraints

- No direct production database writes without a second engineer sign-off
- All schema migrations require a Jira ticket tagged `db-migration` and reviewed by the Staff Engineer
- PagerDuty pages must be acknowledged within 15 minutes (SLA for Enterprise tier customers)
- No customer PII in Slack, Jira comments, or Confluence pages
- All external-facing copy (release notes, API docs) must go through Sam (PM) before publish
