# SKILL.md — Writing Cypress E2E Tests (Dispatch Core)

## When to Use
When writing or reviewing Cypress E2E tests for the field service management platform.
Tests live in `apps/web/cypress/e2e/`.

---

## Test Structure Template

```typescript
describe('[Feature Area] — [Scenario Group]', () => {
  let testTechnician: Technician;
  let testJob: Job;

  beforeEach(() => {
    // Always create isolated test data — never share across tests
    cy.task('createTechnician', { status: 'available' }).then((t) => {
      testTechnician = t;
    });
    cy.task('createJob', { status: 'pending' }).then((j) => {
      testJob = j;
    });
    cy.loginAs('qa-test-dispatcher-1@company.internal');
    cy.visit('/dispatch');
  });

  it('should [expected outcome] when [condition]', () => {
    // Intercept before triggering the action
    cy.intercept('POST', '/api/v1/jobs/*/assign').as('assignJob');

    // Act
    cy.getByTestId('job-card').contains(testJob.id).click();
    cy.getByTestId('assign-technician-btn').click();

    // Wait for network, not time
    cy.wait('@assignJob').its('response.statusCode').should('eq', 200);

    // Assert UI reflects the change
    cy.getByTestId('job-status-badge').should('contain', 'Assigned');
  });
});
```

---

## Selector Rules

| Use | Never use |
|---|---|
| `cy.getByTestId('...')` (custom command wrapping `data-testid`) | CSS class selectors |
| `cy.contains('[data-testid="..."]', 'text')` | Text-only selectors |
| `cy.get('[data-testid="..."]').first()` | XPath |

---

## Network Waiting Rules

```typescript
// CORRECT — intercept before action, wait on alias
cy.intercept('POST', '/api/v1/jobs/*/assign').as('assignJob');
cy.getByTestId('assign-btn').click();
cy.wait('@assignJob');

// NEVER — hardcoded wait
cy.wait(3000);  // ← blocked by lint rule
```

---

## Test Data Rules

- Every test creates its own data via `cy.task()` in `beforeEach`
- Use accounts: `qa-test-technician-[1-20]@company.internal`
- Never use real customer data or production account credentials
- Tests that modify state must be self-contained — staging resets at 2am UTC

---

## Concurrent Scenario Pattern (Playwright)

For race condition testing (e.g. two dispatchers assigning same technician):
```typescript
// Use Playwright multi-page, not Cypress (Cypress is single-tab)
const [result1, result2] = await Promise.all([
  page1.request.post('/api/v1/jobs/123/assign', { data: { technician_id: 'T1' } }),
  page2.request.post('/api/v1/jobs/123/assign', { data: { technician_id: 'T1' } }),
]);
// Exactly one should succeed (200), one should conflict (409)
expect([result1.status(), result2.status()].sort()).toEqual([200, 409]);
```

---

## Anti-Patterns

- `cy.wait([number])` — blocked by lint rule, always use intercept alias
- Shared `testTechnician` fixture across tests — causes flakiness in parallel runs
- Asserting on translated text strings — breaks on locale change
- Not testing the error path — every happy path test needs a paired failure test
