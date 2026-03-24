# Prompt Library — Jordan Lee, QA Engineer

## How to Use This File

**Do not load this file into Claude.** This is a reference for you, not context for Claude.

1. Find the prompt that matches your task
2. Copy it into your Claude Code session
3. Replace the `[PLACEHOLDER]` values with your actual input
4. Run it — Claude already has your context from `CLAUDE.md` and `memory.md`

Adapt prompts over time. If a prompt consistently needs the same tweak, edit this file.

---

## 1. Write Cypress E2E Tests

```
Write Cypress E2E tests for the following feature on the field service management platform.

Tech stack: Cypress, TypeScript, React frontend, Node.js/PostgreSQL backend.
Test location: apps/web/cypress/e2e/

Rules:
- Use data-testid selectors only — no CSS classes, no text selectors
- Use cy.intercept() + cy.wait('@alias') — never cy.wait([number])
- Create isolated test data in beforeEach using cy.task() — no shared fixtures
- Test accounts: qa-test-dispatcher-[N]@company.internal, qa-test-technician-[N]@company.internal
- Cover: happy path, at least one error/failure path, and one edge case
- Test name format: "should [expected outcome] when [condition]"

Feature to test: [DESCRIBE THE FEATURE AND ITS ACCEPTANCE CRITERIA]
API endpoints involved: [LIST ENDPOINTS IF KNOWN]
```

---

## 2. Manual Test Plan

```
Write a manual test plan for the following feature on the field service management platform.

Format for TestRail import:
- Section per test scenario
- Each test case: numbered steps, expected result on every step
- Severity: P1 (data loss/security), P2 (core workflow broken), P3 (degraded), P4 (cosmetic)

Include:
- Happy path (primary flow)
- Error paths (invalid input, server errors, network failure)
- Permission boundary (dispatcher vs. technician vs. admin roles)
- Edge cases (empty state, maximum values, concurrent actions)

Feature: [DESCRIBE THE FEATURE]
Acceptance criteria from Jira (FSM-[XXXX]): [PASTE ACs]
```

---

## 3. Bug Report

```
Write a formal bug report for the following issue found on the field service management platform.

Format:
**Title:** [Concise, factual — what broke, where]
**Severity:** [P1/P2/P3/P4]
**Environment:** [staging/production], browser/version
**Steps to Reproduce:** [Numbered, exact]
**Expected:** [What should happen per the AC]
**Actual:** [What happened instead]
**Evidence:** [Screenshots/logs placeholder]
**Affected Jira ticket:** FSM-[XXXX]

Rules:
- Factual and neutral — do not assign blame
- Steps must be reproducible by any engineer from scratch
- If found in production, note whether it affects Enterprise tier customers

Issue I found: [DESCRIBE THE BUG]
```

---

## 4. Flaky Test Triage

```
Help me triage a flaky Cypress test on the field service management platform.

Known root causes in our suite:
- Shared test data (same technician ID reused across parallel tests)
- cy.wait([number]) masking timing issues
- Missing cy.intercept before triggering async actions
- Test asserting on DOM before network response completes

Flaky test file: [FILENAME]
Flake rate: [X%]
Failure pattern: [DESCRIBE HOW IT FAILS — always same assertion? random? only in CI?]

Test code:
[PASTE TEST CODE]

Provide: (1) most likely root cause, (2) minimal fix, (3) whether the fix pattern
applies to other tests in the same file.
```

---

## 5. Release Regression Checklist

```
Generate a regression test checklist for the following release on the field service management platform.

Changes in this release (from Jira FSM sprint):
[LIST JIRA TICKETS AND BRIEF DESCRIPTIONS]

For each change, list:
- Smoke tests to run (can it open, can it save)
- Regression risk areas (what existing functionality could break)
- Integration points to verify (Mobile squad, Data Platform events, API consumers)

Also include standard regression items:
- Job lifecycle: pending → assigned → en_route → on_site → complete
- SLA breach detection
- Dispatcher and technician role permissions
- Concurrent assignment conflict handling

Format as a checklist for TestRail import.
```

---

## 6. API Contract Test (Playwright)

```
Write a Playwright API contract test for the following endpoint on the field service
management platform.

Use Playwright's APIRequestContext (not the browser UI).
Assert on: HTTP status code, response body schema (required fields + types),
error response for invalid input, and authentication rejection (401 without token).

Endpoint: [METHOD] [PATH]
Expected response schema: [DESCRIBE OR PASTE SCHEMA]
Known edge cases: [LIST ANY KNOWN FAILURE MODES]
Auth: Bearer token — use qa-test-dispatcher-1@company.internal credentials from env
```
