# memory.md — Alex Rivera, Full Stack Engineer

## Project Context

**Project Falcon (FSM-1201):**
- Event-driven redesign of the dispatch engine — RabbitMQ replacing polling
- Branch: `feature/falcon-dispatch-events`
- May 30 ship date is hard — tied to a contract commitment to a key enterprise customer (TechServ Group)
- The mobile squad (iOS/Android) is a downstream consumer of dispatch events — must be notified 2 weeks before any event schema change

**Enterprise API v2 (FSM-1355):**
- Chris Wang (BA) owns requirements; I own implementation
- REST + webhooks, versioned (`/api/v2/`)
- Target June 15
- Webhook retry logic is a known gap — not yet designed

## Domain Rules

- All job statuses flow: `pending → assigned → en_route → on_site → complete | cancelled`
- Status transitions are enforced in `src/domain/job/status-machine.ts` — do not bypass
- Technician location updates fire every 30 seconds from the mobile app
- `dispatch_jobs.scheduled_at` is always stored in UTC; display conversion is the frontend's responsibility
- SLA breach calculation lives in `src/services/sla/calculator.ts` — used by both Dispatch Core and Data Platform (Priya's models read this logic)

## People & Relationships

- **Jordan Lee (QA):** Thorough and methodical. Will block a merge for missing test coverage. Always loop in early on anything touching the `job_assignment` module.
- **Sam Okafor (PM):** Writes tight acceptance criteria. Goes to Sam first when scope is ambiguous — do not make product decisions unilaterally.
- **Morgan Blake (Ops):** Owns production runbooks. If a change affects error codes or log formats, tell Morgan before shipping — they maintain the Datadog alert rules.
- **Priya Nair (Data):** Consumes `dispatch_events` Kafka topic downstream. Schema changes to events need a heads-up to Priya at least 1 sprint ahead.
- **Staff Engineer (Ravi Sharma):** Must review any architectural decision (e.g., Redis vs. RabbitMQ). Don't ship architecture choices without his sign-off.

## Incidents & Lessons

- **March 2 incident:** A missing DB index on `dispatch_jobs.technician_id` caused a 4-minute query timeout during peak dispatch load. Now: always run `EXPLAIN ANALYZE` before merging queries on large tables.
- **Feb 14 incident:** Race condition in `job_assignment` caused double-booking of a technician. Root cause: no locking on concurrent assign calls. Workaround: Redis distributed lock (`src/lib/locks/dispatch.lock.ts`). Proper fix pending Falcon.

## Systems

- GitHub repo: `org/fsm-platform` — monorepo, apps in `/apps/`, shared libs in `/packages/`
- Jira board: FSM — sprints are 2 weeks, planning every other Monday
- SonarQube: `sonar.company.internal` — coverage reports linked in every PR
- Staging env: `staging.fsm.internal` — resets nightly at 2am UTC
- Production deploy: manual approval required in GitHub Actions after all checks pass
