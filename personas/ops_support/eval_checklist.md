# Eval Checklist — Morgan Blake, Senior Support Engineer

Use after Claude produces output. Score each section and decide: use as-is / revise / reject.

---

## Runbook Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Every diagnosis step has a concrete action (exact Datadog query or command) | | | |
| Every decision point has an explicit Yes/No branch with conditions | | | |
| State-changing steps are preceded by a CAUTION block | | | |
| Escalation section names: who, when (time threshold), what to have ready | | | |
| No internal service names in customer-facing language sections | | | |
| Post-incident checklist is included | | | |
| Last updated date and runbook ID are present | | | |

**Score threshold:** 7/7. Any write step without CAUTION = reject immediately.

---

## Post-Mortem Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Timeline uses UTC timestamps | | | |
| Tone is blameless — describes what, not who | | | |
| Root cause is stated factually | | | |
| Customer impact uses tier, not customer name | | | |
| Action items have owner + due date or Jira ticket | | | |
| "What went well" and "what went wrong" are both present | | | |

**Score threshold:** 6/6. Any blame language or customer name = reject immediately.

---

## Zendesk Response Quality

| Check | Pass | Revise | Reject |
|---|---|---|---|
| Acknowledges customer impact in the first sentence | | | |
| Contains no internal service names or team names | | | |
| Does not speculate on root cause | | | |
| Does not commit to a specific resolution time unless confirmed | | | |
| Ends with a specific next action or update time | | | |
| Tone is professional and empathetic | | | |

**Score threshold:** 6/6. Internal service name or speculative root cause = reject.

---

## Iteration Log

| Date | Task | Prompt quality (1–5) | What worked | What to change next time |
|---|---|---|---|---|
| | | | | |
