# Safety Checklist — Chris Wang, Business Analyst

Use this checklist before acting on any Claude output that touches requirements, specs, or customer data.

---

## Before Using Any Generated Requirements

- [ ] Does the document use "must" or "will" instead of "shall" or "should"?
- [ ] Is any requirement unverifiable (a tester cannot write a test case from it)?
- [ ] Does any requirement specify implementation rather than behaviour?
- [ ] Is any requirement missing a source trace (Jira ticket, customer request, product decision)?
- [ ] Are error handling requirements missing for any endpoint (401, 429, 400, 500)?
- [ ] Do all timestamps specify ISO 8601 UTC?
- [ ] Are rate limits specified per customer tier?

**If any box is checked → revise before sharing with engineering.**

---

## Before Sharing a Spec With Engineering

- [ ] Has Sam Okafor reviewed and approved the scope?
- [ ] Are all open questions assigned an owner and due date?
- [ ] Does the spec include a backwards-compatibility note for v1 API consumers?
- [ ] Has Alex Rivera confirmed feasibility of any technically complex requirement?
- [ ] Does the spec contain any customer names or PII? (Remove before sharing)

---

## Before Publishing to Confluence

- [ ] Have you personally reviewed the full document?
- [ ] Is the document status set correctly (Draft / In Review / Approved)?
- [ ] Are requirement IDs (REQ-API2-NNN) sequential and non-duplicated?
- [ ] Has any requirement that touches auth or rate limiting been confirmed with Alex?

---

## Before Finalising a Data Mapping

- [ ] Is the transformation rule explicit for every field — not just "copy"?
- [ ] Are PII fields flagged and not populated with real customer data examples?
- [ ] Are new v2 fields clearly distinguished from v1 fields?
- [ ] Has Morgan Blake been notified of any new error codes or webhook payload fields?

---

## Red Lines — Never Do These

- Never publish to Confluence without your own review
- Never make product decisions — document, flag, refer to Sam Okafor
- Never include customer names, emails, or account IDs in any spec document
- Never commit to scope, timeline, or technical approach on behalf of Sam or Alex
- Never create or close Jira tickets autonomously
