# Safety Checklist — Jordan Lee, QA Engineer

Use this checklist before acting on any Claude output that touches tests, bug reports, or release decisions.

---

## Before Using Any Generated Test Code

- [ ] Do any selectors use CSS classes or text content instead of `data-testid`?
- [ ] Does any test use `cy.wait([number])`? (Must use `cy.intercept` + `cy.wait('@alias')`)
- [ ] Does the test use shared fixtures instead of creating data in `beforeEach`?
- [ ] Does any test account use real customer email addresses? (Must use `qa-test-*@company.internal`)
- [ ] Does the test only cover the happy path? (Every test needs at least one error path)
- [ ] Are concurrent scenarios tested with Playwright, not Cypress? (Cypress is single-tab)

**If any box is checked → revise before committing.**

---

## Before Submitting a Test Plan to TestRail

- [ ] Does the plan cover: happy path, error paths, permission boundaries, empty state?
- [ ] Does every step have an explicit expected result?
- [ ] Are severity levels assigned (P1–P4)?
- [ ] Has the plan been reviewed against Sam Okafor's acceptance criteria in Jira?
- [ ] Are concurrent assignment scenarios included for any dispatch-related feature?

---

## Before Approving a Release

- [ ] Has the full regression suite passed in CI?
- [ ] Have the top 3 known flaky tests been manually re-run and confirmed stable?
- [ ] Has Alex Rivera confirmed no last-minute changes to `job_assignment` or `status-machine`?
- [ ] Has Sam Okafor confirmed all ACs are met?
- [ ] Is this sign-off explicitly requested — not assumed from a passing CI run?

---

## Before Escalating a Bug

- [ ] Is the bug reproducible from the steps in the report?
- [ ] Has severity been assigned accurately (P1 = data loss/security, P2 = core workflow broken)?
- [ ] If P1 in production and affecting Enterprise: has Sam Okafor been notified?

---

## Red Lines — Never Do These

- Never trigger CI pipeline or approve a production deploy
- Never close a TestRail test run autonomously
- Never use real customer data in test fixtures
- Never agree to skip regression without Lena Brandt's explicit approval
- Never mark a release "ready to ship" based solely on CI passing
