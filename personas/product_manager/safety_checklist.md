# Safety Checklist — Sam Okafor, Product Manager

Use this checklist before acting on any Claude output that touches specs, communications, or decisions.

---

## Before Using Any Generated Document

- [ ] Does the document commit to a specific ship date without Alex Rivera confirming feasibility?
- [ ] Does the document reference any feature marked "under consideration" as confirmed?
- [ ] Does the document contain customer names, company names, or contact details?
- [ ] Does the document contain pricing information?
- [ ] Is this going to an external audience — has it been reviewed by you before use?

**If any box is checked → revise before using.**

---

## Before Using Generated Acceptance Criteria

- [ ] Does every AC use Given/When/Then format?
- [ ] Does every AC include at least one error or failure path?
- [ ] Are permission boundaries explicit (dispatcher vs. technician vs. admin)?
- [ ] Is empty state behaviour covered?
- [ ] Are there any vague terms like "notify users" without specifics?

---

## Before Sending a Stakeholder Update

- [ ] Is the RAG status (Red/Amber/Green) accurate — not optimistic?
- [ ] If Red or Amber: has Rachel Kim been notified before this goes out?
- [ ] If customer-facing: have internal team names and service names been removed?
- [ ] Does the update contain any commitment that needs engineering confirmation first?

---

## Before Publishing Release Notes or External Content

- [ ] Have you personally reviewed this draft — not just approved Claude's output?
- [ ] Is it benefit-first language — no engineering jargon?
- [ ] Are there any references to future features or the roadmap?
- [ ] Are there any Jira ticket numbers visible to customers?
- [ ] If API documentation: has Alex Rivera reviewed for technical accuracy?

---

## Red Lines — Never Do These

- Never let any output commit to a ship date without engineering sign-off
- Never reference unreleased or "under consideration" features as confirmed
- Never publish external content without your review
- Never share customer PII in any document or communication
- Never create or close Jira tickets through Claude autonomously
