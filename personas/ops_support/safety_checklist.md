# Safety Checklist — Morgan Blake, Senior Support Engineer

Use this checklist before acting on any Claude output that touches runbooks, incidents, or customer communications.

---

## Before Using Any Generated Runbook Step

- [ ] Does any step modify production state (write to DB, restart service, change config)?
  → If yes: is there a CAUTION block and a confirmation check before it?
- [ ] Does every diagnosis step have a concrete action (exact Datadog query, not "check logs")?
- [ ] Does every decision point have an explicit Yes/No branch?
- [ ] Does the escalation section name: who to page, when, and what to have ready?
- [ ] Does the runbook contain internal service names in customer-facing language?

**If any box is checked → revise before adding to the runbook library.**

---

## Before Running Any Suggested Operational Action

- [ ] Is this a read-only action? If write: has Alex Rivera + Staff Engineer signed off?
- [ ] Are you running this on staging or production? (Production = extra caution)
- [ ] Is this action reversible? If not: do you have a rollback plan?
- [ ] If restarting a service: have you confirmed the consumer is actually stopped first?

---

## Before Sending a Customer Response (Zendesk)

- [ ] Does the draft acknowledge the customer's impact in the first sentence?
- [ ] Does the draft contain any internal service names, team names, or architecture details?
- [ ] Does the draft speculate on root cause without confirmation?
- [ ] Does the draft commit to a specific resolution time that hasn't been confirmed?
- [ ] Have you personally reviewed and approved this before sending?

---

## Before Closing an Incident

- [ ] Has the root cause been confirmed — not assumed?
- [ ] Has the Jira incident ticket been updated with the timeline?
- [ ] Is a post-mortem required? (Required for all P1s, optional for P2s >2h)
- [ ] Have any runbook gaps found during the incident been logged for update?
- [ ] Has Priya Nair's SLA dashboard been checked to quantify customer impact?

---

## Red Lines — Never Do These

- Never run write commands on production infrastructure autonomously
- Never acknowledge or close PagerDuty pages without Morgan's explicit action
- Never send a Zendesk reply without reviewing the draft first
- Never handle a suspected security incident through the normal process — use `#security-incidents`
- Never share internal service names or architecture in external communications
