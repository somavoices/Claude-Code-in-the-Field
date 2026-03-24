# memory.md — Jordan Lee, QA Engineer

## Project Context

**Project Falcon E2E Coverage (FSM-1201):**
- Falcon introduces WebSocket-based real-time dispatch updates — new test pattern needed
- Cypress doesn't natively intercept WebSocket frames — using `cy.intercept` on the REST
  fallback + asserting DOM updates within `cy.wait` timeout
- Race condition risk: two dispatchers assigning the same technician simultaneously
  — test plan includes concurrent session simulation using Playwright multi-page
- May 30 ship date is contractually committed (TechServ Group) — no slip room

**Flaky Tests (FSM-1301):**
- Top 3 flakiest: `dispatch_assign_spec.cy.ts` (31% flake), `job_status_transition_spec.cy.ts`
  (24% flake), `technician_map_spec.cy.ts` (19% flake)
- Root cause for most: shared test data (same technician ID used across parallel test runs)
- Fix pattern: each test creates a unique technician via `cy.task('createTechnician')`
  in `beforeEach`, not shared fixture

## Domain Rules

- Job statuses: `pending → assigned → en_route → on_site → complete | cancelled`
  — status transitions enforced server-side; test invalid transitions too
- Technician must be `available` before assignment — test the unavailable path
- All times in test data must be UTC — staging env is UTC, CI runners are UTC
- Test accounts: `qa-test-technician-[N]@company.internal`,
  `qa-test-dispatcher-[N]@company.internal` — N is 1–20
- Staging resets nightly at 2am UTC — tests that depend on persistent state must
  account for this

## People & Relationships

- **Sam Okafor (PM):** Writes acceptance criteria in Jira. Sam's ACs are the
  authoritative definition of done — if a test contradicts an AC, raise with Sam before
  changing the test.
- **Alex Rivera (Dev):** Reliable, communicates schema and API changes ahead of time.
  When Alex says "might need a new test for this," it always does.
- **Morgan Blake (Ops):** Reports production anomalies in Zendesk or Slack. When Morgan
  flags something odd in prod, check if there's a missing test coverage gap first.
- **Lena Brandt (EM):** Engineering Manager. Escalate to Lena if asked to skip regression
  for a deadline — do not agree to skip without her explicit approval.

## Incidents & Lessons

- **March 10 — Dispatch double-book in staging:** Test suite missed a concurrent assignment
  race condition because tests were running sequentially. Now: concurrent dispatch scenarios
  added to Playwright suite using `Promise.all` for parallel page actions.
- **Feb 3 — `cy.wait(3000)` epidemic:** 12 tests failed in CI after infrastructure
  upgrade reduced response times. Root cause: hardcoded `cy.wait(3000)` calls that
  masked timing issues. Fixed all 12; added lint rule to block `cy.wait([number])`.

## Systems

- Cypress: `apps/web/cypress/` — specs in `e2e/`, support in `support/`
- Playwright: `apps/api-tests/` — contract tests per endpoint
- TestRail: `testrail.company.internal` — project: FSM Platform, suites by feature area
- GitHub Actions: E2E runs on every PR, full regression on main merge
- Datadog: flaky test dashboard in progress — `datadog.company.internal/dashboard/qa-flakiness`
- Staging: `staging.fsm.internal` — resets 2am UTC nightly
