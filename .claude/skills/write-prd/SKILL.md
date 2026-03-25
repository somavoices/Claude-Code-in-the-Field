---
name: write-prd
description: Write a Product Requirements Document (PRD) for Dispatch Core. Uses Given/When/Then acceptance criteria, requires success metrics, and enforces explicit scope boundaries.
allowed-tools: Read, Write
---

# Writing Feature Specifications (PRDs)

## When to Use
When writing a product requirements document (PRD) or feature spec for the
Dispatch Core squad. Used for Jira epics that go to engineering for estimation.

---

## PRD Structure

```markdown
# [Feature Name] — Product Requirements

**Jira Epic:** FSM-[XXXX]
**Status:** Draft / In Review / Approved
**PM:** Sam Okafor
**Last updated:** [DATE]

---

## Problem Statement
[1–3 sentences: what customer problem are we solving, and why now?
Lead with customer pain, not the solution.]

## Success Metrics
- [Metric 1]: current [X] → target [Y] by [date]
- [Metric 2]: current [X] → target [Y] by [date]

## Scope

### In Scope
- [Bullet list of what is included]

### Out of Scope
- [Explicit list — avoids scope creep]

## User Stories

### Story 1: [Title]
**As a** [dispatcher / technician / admin]
**I want to** [action]
**So that** [outcome]

#### Acceptance Criteria
- **Given** [initial context]
  **When** [action is taken]
  **Then** [expected result]
- **Given** [error condition]
  **When** [action is taken]
  **Then** [expected error handling]

## Edge Cases & Constraints
- [List known edge cases engineering must handle]
- [State any technical constraints flagged by Alex Rivera]

## Dependencies
- [List other squads, features, or decisions this depends on]
- Mobile squad notification required? [Yes/No — if yes, 2-week heads-up needed]

## Open Questions
| Question | Owner | Due |
|---|---|---|
| [Question] | [Name] | [Date] |
```

---

## Acceptance Criteria Rules

1. Every AC uses **Given/When/Then** — no "should" or "must" without a condition
2. Every AC has an **error path** — not just the happy path
3. **Permission boundaries** explicit: what can a dispatcher do that a technician cannot?
4. **Empty state** covered: what does the user see when there is no data?
5. Never use vague language: "notify users" → specify channel, timing, content

---

## Anti-Patterns

- Writing solutions in the problem statement ("We need a button that...") — describe the pain first
- Open questions left unresolved at sprint planning — resolve or explicitly defer before handoff
- ACs that only cover the happy path — Jordan Lee will flag the gaps in test planning
- Committing a ship date in the PRD without Alex Rivera confirming feasibility
- Referencing features marked "under consideration" as confirmed
