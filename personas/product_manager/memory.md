# memory.md — Sam Okafor, Product Manager

## Project Context

**Project Falcon (FSM-1201):**
- Ship date May 30 — hard commitment in TechServ Group's renewal contract
- Success criteria: dispatch re-route latency <500ms p95, zero double-bookings in first 30 days
- Beta customers: TechServ Group, GridLink Services (both Enterprise tier, both briefed)
- Open risk: Mobile squad hasn't confirmed they can consume new WebSocket events in time
  — follow-up with Mobile PM (Dani Cho) by April 10
- Jordan Lee owns the test plan; Alex Rivera owns the architecture decision (Redis vs. RabbitMQ)

**Enterprise API v2 (FSM-1355):**
- Chris Wang leading BA requirements — I review before passing to engineering
- Scope agreed: REST versioning (/api/v2/), webhook support for job status changes,
  rate limiting per customer tier
- NOT in scope (agreed with Chris and VP Eng): GraphQL, real-time streaming, OAuth2 (Q3)
- Target: June 15

## Domain Rules

- Never commit a ship date without Alex confirming engineering feasibility
- The Mobile squad (Dani Cho) must be notified 2 weeks before any breaking API change
- Enterprise customers are on a 4-hour SLA — any feature affecting dispatch latency
  requires Morgan Blake's review before release
- All external-facing copy (release notes, API docs, in-app messaging) goes through
  me before publish
- Roadmap items marked "under consideration" cannot be referenced in customer calls
  as confirmed — sales has burned us on this twice

## People & Relationships

- **Alex Rivera (Dev):** Trusted, direct. Will push back on scope if timeline is unrealistic.
  When Alex says "that needs more time," it always does — don't override.
- **Jordan Lee (QA):** Thorough. Jordan's test plan sign-off is my gate for release-ready.
  Do not ship without Jordan's green light.
- **Priya Nair (Data):** Builds the exec dashboard I use for OKR tracking. If numbers look
  off, go to Priya before escalating — usually a data pipeline issue, not a product problem.
- **Morgan Blake (Ops):** Brings real customer pain from Zendesk. Morgan's escalations are
  signal — when he flags a pattern, it goes on the next sprint.
- **Rachel Kim (VP Product):** My manager. Needs weekly status updates on Falcon. Prefers
  one paragraph + RAG status (Red/Amber/Green) over long reports. Escalate Falcon risk
  to Rachel immediately — she has the executive relationship with TechServ.
- **Dani Cho (Mobile PM):** Owns the iOS/Android app. Must align with her on any API
  change that affects the mobile app — she's a stakeholder, not a downstream consumer.

## Incidents & Lessons

- **Q1 — Scope creep on Notifications feature:** Engineering started building push
  notifications that weren't in the agreed spec. Added 3 weeks to the timeline.
  Root cause: vague AC ("should notify users"). Fix: all ACs now use Given/When/Then.
- **Feb — Sales referenced unannounced bulk dispatch feature on a customer call.**
  Customer expected it in 30 days. Had to walk back. Now: all "under consideration"
  roadmap items are marked explicitly and not shared in customer-facing decks.

## Systems

- Jira board: FSM — I triage the backlog every Monday, sprint planning every other Monday
- Confluence: product specs and PRDs at `confluence.company.internal/display/PROD`
- Figma: design files linked from each Jira epic
- Looker: OKR dashboard shared with Rachel Kim — updated by Priya weekly
- Slack: `#dispatch-core` (squad), `#product` (PM team), `#escalations` (Morgan's alerts)
