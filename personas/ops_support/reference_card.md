# Reference Card — Morgan Blake, Senior Support Engineer

## How to Use This File

**Do not load this file into Claude.** Keep it open alongside your Claude Code session.

- Before writing a prompt: check the 4-part structure and context shortcuts
- When a prompt produces weak output: use the vague vs. specific examples to diagnose why
- When iterating: use the iteration patterns section to identify what to fix

---

## Prompt Engineering Best Practices

### The 4-Part Prompt Structure
```
[CONTEXT]   Incident type + affected service + severity + customer tier
[TASK]      What operational artefact you need — runbook, post-mortem, response, monitor
[RULES]     Concrete steps only, no internal names externally, read-only by default, blameless
[INPUT]     Incident timeline, logs, Datadog metrics, or Zendesk ticket
```

### Make Prompts Specific to Your Context
| Vague | Specific |
|---|---|
| "Write a runbook" | "Write a runbook for RabbitMQ consumer not running — symptoms in Datadog, safe restart steps, escalation to Alex Rivera if unresolved in 10 min" |
| "Write a post-mortem" | "Write a blameless post-mortem for a 2.5h P1 dispatch outage — timeline provided, root cause: missing consumer after deploy, 3 Enterprise customers affected" |
| "Reply to this ticket" | "Draft a Zendesk response acknowledging a webhook delivery failure for an Enterprise customer — issue is resolved, do not share internal service names" |

### Context Shortcuts
- **Platform:** "Field service management — dispatch engine, webhooks, API auth — monitored in Datadog"
- **SLAs:** "Enterprise 4h (page 15 min), Business 8h (page 30 min), Starter 24h (email)"
- **Rules:** "Read-only steps only, CAUTION before any state change, escalate P1 to Alex Rivera via PagerDuty"

---

## Claude for Daily Ops Work

| Task | Best Prompt Type | Key Rule |
|---|---|---|
| Runbook | Describe incident + service + safe fix | Every step must be executable |
| Post-mortem | Paste timeline + root cause | Blameless, UTC timestamps |
| Zendesk response | Paste ticket + known facts | Acknowledge impact first, no internal names |
| Datadog monitor | Describe alert condition | P1 must page PagerDuty, include runbook link |
| Runbook review | Paste runbook | Check for concrete steps + escalation paths |
| SLA summary | Paste dashboard numbers | Lead with business impact, exact numbers |

---

## Iteration Patterns

- **Step says "check logs"** → "Replace with exact Datadog log query for this service"
- **No escalation path** → "Add: who to page, when (time threshold), what to have ready"
- **Customer response has internal names** → "Remove all internal service/team names — use 'our platform'"
- **Post-mortem assigns blame** → "Rewrite to describe what happened, not who caused it"

---

## Safety Reminders

- Never run write commands on production — read-only only
- Never acknowledge or close a PagerDuty page without Morgan's explicit action
- Never send a Zendesk reply without reviewing the draft first
- If you suspect a security incident: use `#security-incidents` — do NOT use normal incident process
