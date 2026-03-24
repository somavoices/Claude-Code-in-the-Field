# Prompt Library — Morgan Blake, Senior Support Engineer

## How to Use This File

**Do not load this file into Claude.** This is a reference for you, not context for Claude.

1. Find the prompt that matches your task
2. Copy it into your Claude Code session
3. Replace the `[PLACEHOLDER]` values with your actual input
4. Run it — Claude already has your context from `CLAUDE.md` and `memory.md`

Adapt prompts over time. If a prompt consistently needs the same tweak, edit this file.

---

## 1. Write an Operational Runbook

```
Write an operational runbook for the following incident type on the field service
management platform (B2B SaaS, Platform & Reliability squad).

Format:
- Symptoms table: customer-reported symptom + Datadog metric/log pattern
- Severity assessment table: conditions → P1/P2/P3 + SLA impact
- Diagnosis steps: each step has a concrete Datadog query or action (not "check logs")
- Resolution steps: Fix A (safe/self-service), Fix B (escalate to engineering)
- CAUTION block before any step that modifies state
- Escalation section: who to page, when, what to have ready
- Post-incident checklist

Rules:
- Every step must be executable without asking a question
- Write commands preceded by CAUTION — default is read-only
- Do not use internal service names in customer-facing language
- Escalation must name: who (Alex Rivera for dispatch, Lena Brandt for escalation),
  when (time threshold), and what to have ready (correlation ID, Datadog link)

Incident type to document: [DESCRIBE THE INCIDENT / FAILURE MODE]
Affected service: [SERVICE NAME]
Relevant Datadog metrics: [LIST IF KNOWN]
```

---

## 2. Incident Post-Mortem

```
Write an incident post-mortem for the following production incident on the
field service management platform.

Format:
1. Incident Summary (1 paragraph: what happened, who was affected, how long)
2. Timeline (chronological, UTC timestamps)
3. Root Cause (technical, factual — no blame)
4. Customer Impact (by tier: Enterprise / Business / Starter)
5. What Went Well
6. What Went Wrong
7. Action Items (owner, due date, Jira ticket)

Rules:
- Factual and blameless — describe what happened, not who caused it
- Timeline must be in UTC
- Customer impact: use tier, not customer name
- Action items must have an owner (name) and a Jira ticket or due date
- Do not speculate on impact — use Priya Nair's SLA dashboard numbers if available

Incident details:
- Date/time: [UTC]
- Duration: [X hours Y minutes]
- Severity: [P1/P2]
- What happened: [DESCRIBE]
- Timeline notes: [PASTE RAW TIMELINE]
- Root cause (as understood): [DESCRIBE]
```

---

## 3. Zendesk Customer Response

```
Draft a Zendesk response to the following Enterprise customer support ticket for the
field service management platform.

Tone: Professional, empathetic, clear. Acknowledge the impact first.
Rules:
- Do not speculate on root cause — only state what is confirmed
- Do not share internal service names, team names, or architecture details
- Do not commit to a specific resolution time unless confirmed with engineering
- If the issue is resolved: state what was fixed and confirm it is resolved
- If still in progress: state what we are doing and when you will next update
- End with a specific next action or next update time

This is a DRAFT — Morgan Blake will review before sending.

Ticket content: [PASTE CUSTOMER TICKET]
Current status: [RESOLVED / IN PROGRESS]
Known facts: [WHAT IS CONFIRMED — root cause, affected scope, resolution if done]
```

---

## 4. Datadog Monitor Setup

```
Write a Datadog monitor configuration for the following alert on the field service
management platform.

Requirements:
- Monitor type: [metric alert / log alert / APM]
- Alert threshold: [specify — e.g., queue depth >1000 for >5 min]
- Warning threshold: [specify]
- Notification: PagerDuty (`FSM Platform On-Call`) for P1, Slack `#ops-alerts` for warning
- Message must include: what triggered, Datadog dashboard link, runbook link (RBK-[NNN])
- Tags: `env:prod`, `service:[service-name]`, `severity:[p1/p2]`
- Evaluation window: [specify]

Rules:
- P1 monitors must page PagerDuty — Slack only is not sufficient for P1
- Include a "no-data" alert if the metric going silent is also a failure mode
- Runbook link must be included in the alert message

Alert to create: [DESCRIBE WHAT CONDITION SHOULD TRIGGER THE ALERT]
Service: [SERVICE NAME]
Relevant metric or log pattern: [DESCRIBE]
```

---

## 5. Runbook Review Checklist

```
Review the following runbook for the field service management platform before
raising a PR for Lena Brandt's review.

Check for:
- Every diagnosis step has a concrete action (exact Datadog query or command)
- Every decision point has an explicit Yes/No branch with conditions
- Write or state-changing steps have a CAUTION block
- Escalation section names: who, when (time threshold), what to have ready
- No internal service names in any customer-facing language sections
- Last updated date is present
- Post-incident checklist is included
- Runbook ID (RBK-NNN) is assigned

Runbook:
[PASTE RUNBOOK HERE]
```

---

## 6. Weekly SLA Breach Summary

```
Write a weekly SLA breach summary for the Dispatch Core squad review (Sam Okafor + Rachel Kim).

Context:
- SLA thresholds: Enterprise = 4h, Business = 8h, Starter = 24h
- Source: Priya Nair's Looker SLA dashboard (numbers provided below)
- Audience: Sam Okafor (PM) and Rachel Kim (VP Product) — non-technical

Format:
1. Headline number: total breaches this week vs. last week (% change)
2. Breakdown by customer tier
3. Top 3 contributing incident types
4. One recommended action item for next sprint

Rules:
- Lead with the business impact (customers affected), not the technical cause
- Use exact numbers from the dashboard — no rounding
- Do not include customer names — use tier only
- Flag any week-over-week increase >20% as a risk item

Dashboard numbers this week: [PASTE NUMBERS FROM LOOKER]
Last week numbers: [PASTE FOR COMPARISON]
```
