# memory.md — Morgan Blake, Senior Support Engineer

## Project Context

**MTTR Reduction Initiative:**
- Baseline: P1 MTTR = 3.2h (Q1 average). Target: <90 min by end of Q2.
- Top 5 P1 incident types by frequency (last 90 days):
  1. Dispatch engine queue backup (8 incidents) — runbook exists but stale
  2. Webhook delivery failures — Enterprise customers (6 incidents) — no runbook
  3. API auth service timeout (5 incidents) — runbook exists, accurate
  4. Fivetran sync failure causing stale Looker data (4 incidents) — Priya owns runbook
  5. Mobile app push notification failure (3 incidents) — Mobile squad owns runbook
- My ownership: items 1, 2, 3

**Runbook Library (FSM-1501):**
- Repo: `org/fsm-runbooks` — 28 runbooks, 11 stale
- Sprint 1 target (done): dispatch engine queue backup updated
- Sprint 2 target (current): webhook delivery failures (new), API auth timeout (update)
- Stale runbooks that are P1 risk: `webhook_delivery_failure.md` (doesn't exist yet),
  `dispatch_queue_backup.md` (updated this sprint)

## Domain Rules

- SLA by customer tier: Enterprise = 4h (page in 15 min), Business = 8h (page in 30 min),
  Starter = 24h (email only)
- PagerDuty escalation: acknowledge within 15 min for Enterprise, 30 min for Business
- Production DB: read-only access only — any write requires Alex Rivera + Staff Engineer sign-off
- Security incidents: do NOT use normal incident process — use `#security-incidents` Slack
  channel and page the Security lead (not on-call rotation)
- Never share internal service names (`dispatch-service`, `auth-service`) in Zendesk or
  external comms — use "our platform" or "the affected service"

## People & Relationships

- **Alex Rivera (Dev):** Key partner for dispatch incidents. Alex is responsive in
  `#dispatch-core` Slack. For P1s: page Alex via PagerDuty if not responding in 10 min.
  Alex will always have the relevant logs — ask for the correlation ID.
- **Sam Okafor (PM):** Escalate customer-impacting P1s to Sam if the customer is
  Enterprise tier. Sam manages the TechServ Group relationship directly.
- **Priya Nair (Data):** Owns the SLA dashboard. If I see numbers that don't add up
  in post-mortem, Priya is the first call. Priya also pages me when source freshness
  fails (she set up the alert).
- **Chris Wang (BA):** Sends me integration questions — my Zendesk history is often
  the only record of undocumented customer integration behaviour.
- **Lena Brandt (EM):** My Engineering Manager. Escalate to Lena for any incident
  that requires an external customer communication or executive briefing.

## Incidents & Lessons

- **March 14 — TechServ Group dispatch outage (P1, 2.5h):**
  Root cause: RabbitMQ queue backed up due to a missing consumer after a deploy.
  Alex identified via Datadog APM. Fix: consumer restarted, queue drained.
  Lesson: added Datadog monitor for queue depth >1000 messages → PagerDuty page.
- **Feb 20 — Webhook delivery silent failure:**
  Webhooks stopped delivering for 3 Enterprise customers for 6 hours — no alert fired.
  Root cause: retry queue exhausted silently. Lesson: added DLQ (dead letter queue)
  monitor → page when DLQ depth >0 for >10 min.

## Systems

- Datadog: `datadog.company.internal` — key dashboards: `FSM Platform Overview`,
  `Dispatch Engine Health`, `API Latency`
- PagerDuty: `pagerduty.company.internal` — on-call schedule: `FSM Platform On-Call`
- Zendesk: `support.company.internal` — views: `Escalations`, `Enterprise Open`
- Runbook repo: `org/fsm-runbooks` on GitHub — PRs reviewed by Lena Brandt
- Confluence: internal incident post-mortems at `confluence.company.internal/display/OPS`
