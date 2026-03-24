# Eval Checklist — Sam Okafor, Product Manager

Use after Claude produces output. Score each section and decide: use as-is / revise / reject.

---

## PRD / Feature Spec Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Problem statement describes customer pain — not the solution | | | |
| Success metrics have current baseline + target + date | | | |
| Out of scope list is explicit | | | |
| ACs use Given/When/Then format | | | |
| Every AC has at least one error path | | | |
| Permission boundaries are explicit | | | |
| Empty state behaviour is covered | | | |
| No ship date committed without engineering confirmation | | | |
| No unreleased features referenced as confirmed | | | |
| No customer PII present | | | |

**Score threshold:** 9/10. Ship date commitment or PII = reject immediately.

---

## Stakeholder Update Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Starts with headline and RAG status | | | |
| 3 bullets or fewer | | | |
| Ends with a single next action | | | |
| No internal team names in customer-facing version | | | |
| No scope or date commitments without engineering sign-off | | | |

**Score threshold:** 5/5.

---

## Release Notes Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Benefit-first language — customer can do X now | | | |
| No Jira ticket numbers | | | |
| No engineering jargon (no "API", "endpoint", "schema") | | | |
| No future roadmap references | | | |
| No pricing information | | | |

**Score threshold:** 5/5. Any roadmap hint or pricing = reject immediately.

---

## Iteration Log

| Date | Task | Prompt quality (1–5) | What worked | What to change next time |
|---|---|---|---|---|
| | | | | |
