---
name: write-runbook
description: Write or update an operational runbook for the FSM platform. Enforces concrete steps with exact commands, explicit Yes/No decision branches, CAUTION blocks before write operations, and named escalation paths.
allowed-tools: Read, Write, Grep
---

# Writing Operational Runbooks (Platform & Reliability)

## When to Use
When creating or updating a runbook in `org/fsm-runbooks` for a production incident type
on the field service management platform.

---

## Runbook Structure

```markdown
# Runbook: [Incident Type Title]

**Runbook ID:** RBK-[NNN]
**Last updated:** [DATE]
**Owner:** Morgan Blake
**Reviewed by:** Lena Brandt (EM)
**Applies to:** [Service/Component name]

---

## Symptoms

| What the customer reports | What Datadog shows |
|---|---|
| [Customer-facing symptom] | [Specific metric/log pattern] |

---

## Severity Assessment

| Condition | Severity | SLA Impact |
|---|---|---|
| [Condition A] | P1 | Enterprise customers affected — page immediately |
| [Condition B] | P2 | Degraded — monitor, no immediate page |

---

## Diagnosis Steps

### Step 1 — Confirm the issue
**Action:** Check Datadog dashboard `[Dashboard Name]` for metric `[metric.name]`.
**Command:**
```
datadog query: avg:dispatch.queue.depth{env:prod} > 1000
```
**Expected if issue is present:** Queue depth >1000, trending up.
**If not present:** Skip to [Step 4 — Alternative diagnosis].

### Step 2 — Identify root cause
**Action:** Check application logs for correlation ID.
**Datadog log query:**
```
service:dispatch-service status:error @http.status_code:5*
```
**Look for:** `consumer_not_running` or `queue_timeout` error messages.

---

## Resolution Steps

### Fix A — Restart the consumer (safe, no data loss)
**When to use:** Queue backed up, consumer process not running.
> ⚠️ CAUTION: Confirm consumer is stopped before restarting — do not run if consumer
> is active (will cause duplicate processing).

**Steps:**
1. Confirm in Datadog: `dispatch.consumer.running = 0`
2. In AWS console → ECS → `dispatch-consumer` service → Force new deployment
3. Monitor queue depth — should begin dropping within 2 minutes
4. Confirm in Datadog: queue depth <100 within 5 minutes

### Fix B — Escalate to engineering
**When to use:** Consumer is running but queue still backing up — code-level issue.
1. Page Alex Rivera via PagerDuty (`FSM Platform On-Call` → Secondary)
2. Have ready: Datadog link, correlation ID from logs, time the issue started
3. Do not attempt further changes — hand off to engineering

---

## Escalation

| Condition | Action | Who |
|---|---|---|
| P1 affecting Enterprise customer | Page immediately | Alex Rivera (PagerDuty Secondary) |
| Issue not resolved in 30 min | Escalate | Lena Brandt (EM) |
| Suspected data loss or security issue | Use security process | `#security-incidents` Slack |

---

## Post-Incident

1. Add timeline to incident ticket in Jira (tag `incident`)
2. Write post-mortem in Confluence: `confluence.company.internal/display/OPS/Post-Mortems`
3. If this runbook was incomplete or wrong: update and raise PR for Lena's review
```

---

## Rules

1. Every step has a **concrete action** — not "check logs" but the exact Datadog query
2. Every decision point has an explicit **Yes/No branch**
3. Write commands are preceded by a **CAUTION block** — safe reads only by default
4. Escalation section names **who to page, when, and what to have ready**
5. Never include internal service names in customer-facing language in the runbook
6. Every runbook has a **last updated date** — stale = >3 months without review

---

## Anti-Patterns

- "Monitor the situation" — specify what metric, what threshold, what action
- Steps that require tribal knowledge ("ask Alex what this means") — document the knowledge
- Missing escalation path — every runbook must have a clear "when to escalate" condition
- Runbook that ends at "contact engineering" without saying who and how
