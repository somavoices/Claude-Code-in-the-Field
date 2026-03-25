---
name: write-pr-description
description: Write a GitHub PR description for Dispatch Core engineering. Follows the What/How/Test Coverage/Risk/Jira/Screenshots format with rules for downstream impact flagging.
allowed-tools: Read, Grep, Bash
---

# Writing GitHub PR Descriptions (Dispatch Core Engineering)

## When to Use
When opening or updating a pull request on `org/fsm-platform`. Use for any change
from a single-line fix to a multi-file feature.

---

## PR Description Format

```
## What
[1–3 sentences: what changed and why. Lead with the business reason, not the implementation.]

## How
[Bullet list of key implementation decisions. Include trade-offs if non-obvious.]

## Test Coverage
[What was tested: unit, integration, E2E. Note if coverage increased/decreased.]
- [ ] Unit tests added/updated
- [ ] Integration tests cover the happy path and at least one error path
- [ ] No new SonarQube critical issues

## Risk & Rollback
[What could go wrong. How to roll back if needed — feature flag, revert, migration rollback script.]

## Jira
[FSM-XXXX](https://jira.company.internal/browse/FSM-XXXX)

## Screenshots / Logs
[Include for any UI change or any change to log output / error messages that Morgan (Ops) monitors.]
```

---

## Rules

1. **Lead with the why, not the what.** "Fixes SLA breach miscalculation during overlap shifts" is better than "Updates calculator.ts."
2. **Flag downstream impact explicitly.** If the change touches dispatch events, API contracts, or log formats — call it out and name the affected team (Mobile, Data Platform, Ops).
3. **Every DB migration PR must include a rollback script** in the description or as a linked file.
4. **If coverage dropped**, explain why and link the SonarQube report.
5. **Tag Jordan Lee as reviewer** on anything touching `job_assignment`, `status-machine`, or E2E test paths.
6. **Tag Ravi Sharma (Staff Engineer)** on any architectural change or new external dependency.

---

## Anti-Patterns to Avoid

- "Misc fixes" — every PR needs a clear what and why
- Skipping the Risk section on DB migrations
- Opening a PR against `main` without a linked Jira ticket
- Marking PR ready-for-review before CI passes
