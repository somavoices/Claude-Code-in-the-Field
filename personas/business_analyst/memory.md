# memory.md — Chris Wang, Business Analyst

## Project Context

**Enterprise API v2 (FSM-1355):**
- Requirements due April 25 — engineering estimation sprint starts April 28
- Confirmed in scope: REST versioning, webhook for job status changes, rate limiting
  (Enterprise: 1000 req/min, Business: 200 req/min, Starter: 50 req/min)
- Confirmed out of scope (Sam + VP Eng agreed): GraphQL, streaming, OAuth2
- Webhook payload format: agreed to mirror existing job object schema + `event_type`
  and `occurred_at` fields — Alex confirmed this is feasible without schema changes
- Rate limiting implementation: Alex's team will use Redis token bucket — BA doesn't
  specify implementation, just the behaviour (reject with 429, include `Retry-After` header)

**Customer Integration Mapping:**
- 14 enterprise customers, 9 active integrations documented
- 3 undocumented integrations (TechServ Group, GridLink Services, Vertex FM)
  — reverse-engineering from Morgan's support runbooks and Zendesk tickets
- TechServ Group uses a custom middleware that polls `/api/v1/jobs` every 60 seconds —
  must ensure v2 has a backwards-compatible migration path

## Domain Rules

- Job status values: `pending | assigned | en_route | on_site | complete | cancelled`
  — these are stable; do not propose new statuses without Sam's approval
- All API timestamps in ISO 8601 UTC (`2026-03-24T14:30:00Z`)
- Customer tier determines rate limit AND webhook retry policy
  (Enterprise: 5 retries with exponential backoff; Business/Starter: 3 retries)
- API versioning policy: v1 must remain live for 12 months after v2 GA
- All new endpoints require: authentication (Bearer token), request ID header,
  rate limit headers in response (`X-RateLimit-Limit`, `X-RateLimit-Remaining`)

## People & Relationships

- **Sam Okafor (PM):** Reviews all specs before handoff to engineering. Sam makes
  product decisions — I document and flag, never decide. Sam is stretched on Falcon;
  book a focused review slot, don't send long Confluence drafts on Slack.
- **Alex Rivera (Dev):** Technically sharp, gives clear feasibility verdicts. If Alex
  says a requirement is ambiguous, it is — revise before moving on.
- **Morgan Blake (Ops):** Critical source for undocumented integration behaviour.
  Morgan's runbooks are the best documentation of how integrations actually behave
  in production. Treat Morgan's input as requirements input, not just support context.
- **Priya Nair (Data):** Can pull API usage metrics from Snowflake if I need volume
  data to prioritise endpoints or justify rate limit tiers.

## Incidents & Lessons

- **March — TechServ polling rate issue:** Discovered TechServ polls every 60 seconds
  — not documented anywhere. Found via Morgan's incident report for a 429 spike.
  Lesson: always cross-check Morgan's Zendesk history before finalising API requirements.
- **Q1 — "should" vs. "shall" dispute:** An engineer treated a "should" as optional
  and omitted it. The customer expected it. Now using strict shall/should convention
  with a definition section in every spec document.

## Systems

- Confluence: requirements at `confluence.company.internal/display/INTEG/API-v2`
- Jira: FSM board — integration tickets tagged `integrations`
- Lucidchart: process diagrams linked from Confluence pages
- Postman: API exploration workspace `FSM Platform APIs` — shared with Alex
- Salesforce: customer account data (read-only) — for integration context only
