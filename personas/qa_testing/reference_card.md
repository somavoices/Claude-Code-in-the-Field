# Reference Card — Jordan Lee, QA Engineer

## How to Use This File

**Do not load this file into Claude.** Keep it open alongside your Claude Code session.

- Before writing a prompt: check the 4-part structure and context shortcuts
- When a prompt produces weak output: use the vague vs. specific examples to diagnose why
- When iterating: use the iteration patterns section to identify what to fix

---

## Prompt Engineering Best Practices

### The 4-Part Prompt Structure
```
[CONTEXT]   Feature area + stack (Cypress/Playwright/Jest) + known risks
[TASK]      What test artefact you need — E2E spec, test plan, bug report
[RULES]     data-testid only, no cy.wait([n]), isolated test data, no real customer data
[INPUT]     ACs from Jira, code under test, or bug description
```

### Make Prompts Specific to Your Stack
| Vague | Specific |
|---|---|
| "Write a test for job assignment" | "Write a Cypress E2E test for dispatcher assigning a technician — cover success (200), unavailable technician (409), and unauthenticated (401)" |
| "Write a bug report" | "Write a P2 bug report for job status not updating after assignment — steps from dispatcher login, reproducible in staging" |
| "Write a test plan" | "Write a TestRail test plan for Project Falcon real-time dispatch — include concurrent assignment, WebSocket reconnect, and SLA breach scenarios" |

### Context Shortcuts
- **Stack:** "Cypress E2E, Playwright API, Jest unit — GitHub Actions CI gates every PR"
- **Rules:** "data-testid selectors, cy.intercept not cy.wait([n]), isolated beforeEach test data"
- **Accounts:** "qa-test-dispatcher-[N]@company.internal, qa-test-technician-[N]@company.internal"

---

## Claude for Daily QA Work

| Task | Best Prompt Type | Key Rule |
|---|---|---|
| Cypress E2E test | Paste ACs + endpoint info | Specify happy path + error paths |
| Manual test plan | Paste feature description + ACs | Include permission boundaries |
| Bug report | Describe symptom + steps | Always include expected vs. actual |
| Flaky test triage | Paste test code + failure pattern | State flake rate and CI vs. local |
| API contract test | Paste endpoint + schema | Assert status + body schema + 401 |
| Release checklist | List sprint tickets | Include concurrent + SLA scenarios |

---

## Iteration Patterns

- **Test uses CSS selector** → "Rewrite using data-testid attributes only"
- **Test uses cy.wait(3000)** → "Replace with cy.intercept + cy.wait('@alias') pattern"
- **Missing error path** → "Add test for: unavailable technician, unauthenticated request, server 500"
- **Shared fixture** → "Each test must create its own data in beforeEach — remove shared fixture"

---

## Safety Reminders

- Never use real customer email addresses or data in test fixtures
- Never trigger CI pipeline or approve a production deploy
- Never mark a release "ready to ship" without Jordan's explicit review
- If asked to skip regression to meet a deadline: escalate to Lena Brandt first
