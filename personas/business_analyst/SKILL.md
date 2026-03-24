# SKILL.md — Writing Functional Requirements (API & Integration Specs)

## When to Use
When writing functional requirements for an API endpoint, integration workflow,
or data mapping in the Integrations squad. Output goes to Confluence and is used
by engineering for estimation and build.

---

## Requirements Document Structure

```markdown
# [Feature/Endpoint Name] — Functional Requirements

**Requirement Set:** REQ-API2-[NNN]–[NNN]
**Jira Epic:** FSM-[XXXX]
**Author:** Chris Wang
**Reviewed by:** Sam Okafor (PM)
**Status:** Draft / In Review / Approved
**Last updated:** [DATE]

---

## Definitions

| Term | Definition |
|---|---|
| shall | Mandatory requirement — must be implemented |
| should | Recommended — implement unless there is a documented reason not to |

---

## Overview
[2–3 sentences: what this requirement set covers and why it exists.
Trace to customer need or product decision.]

---

## Functional Requirements

### REQ-API2-001 — [Short Title]
**Requirement:** The system shall [verifiable behaviour].
**Source:** [Jira ticket / customer request / product decision by Sam Okafor]
**Acceptance test:** [One sentence: how a tester confirms this is met]
**Notes:** [Any constraints or clarifications]

### REQ-API2-002 — [Short Title]
...

---

## Exception & Error Handling

| Condition | Expected Behaviour | HTTP Status |
|---|---|---|
| Missing auth token | Return 401 with `error: "unauthorized"` | 401 |
| Rate limit exceeded | Return 429 with `Retry-After` header | 429 |
| Invalid request body | Return 400 with field-level error array | 400 |

---

## Data Mapping

| Source Field | Type | Transformation | Target Field | Notes |
|---|---|---|---|---|
| `job.id` | UUID | None | `job_id` | Required |
| `job.scheduled_at` | timestamp | Convert to ISO 8601 UTC | `scheduled_at` | Required |

---

## Open Questions

| # | Question | Owner | Due |
|---|---|---|---|
| 1 | [Question] | [Name] | [Date] |
```

---

## Rules

1. Every requirement must be **verifiable** — if a tester can't write a test from it, rewrite it
2. Use **shall** for mandatory, **should** for recommended — never "must", "will", "needs to"
3. Every requirement traces to a **source** — customer, Jira ticket, or product decision
4. Every exception path must be **explicitly documented** — not "handle errors appropriately"
5. Data mappings: include transformation rule — "copy field" is not enough if there's a type cast
6. Open questions must have an **owner and due date** — unowned questions block estimation

---

## Anti-Patterns

- "The system should handle errors" — specify which errors, how, with what response
- Requirement that mixes "what" with "how" — BA specifies behaviour, not implementation
- Missing rate limit / auth requirements — every new endpoint needs both
- Referring to internal field names without confirming they match the API contract
- Spec approved without Sam Okafor review
