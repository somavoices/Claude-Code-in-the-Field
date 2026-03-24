# Team Playbook — Cross-Persona Claude Code Workflows

How the six colleagues use Claude Code individually and together. Read this to understand
how work flows across roles, what each persona hands off to the next, and how Claude
supports the whole team — not just individual contributors.

**Company:** Mid-size B2B SaaS, ~300 employees, field service management platform
**Squad focus:** Dispatch Core + Integrations + Data Platform + Platform & Reliability

---

## The Six Colleagues

| Name | Role | Squad | Claude use focus |
|---|---|---|---|
| Alex Rivera | Full Stack Engineer | Dispatch Core | Code review, PR descriptions, SQL, debugging |
| Priya Nair | Senior Data Analyst | Data Platform | dbt models, SQL queries, stakeholder summaries |
| Jordan Lee | QA Engineer | Dispatch Core | Cypress tests, test plans, bug reports |
| Sam Okafor | Product Manager | Dispatch Core | PRDs, ACs, stakeholder updates, release notes |
| Chris Wang | Business Analyst | Integrations | Functional requirements, process flows, data mappings |
| Morgan Blake | Senior Support Engineer | Platform & Reliability | Runbooks, post-mortems, Zendesk responses |

---

## Feature Delivery Workflow (Project Falcon Example)

How a feature moves through the team — and where Claude helps at each stage.

```
DISCOVERY & REQUIREMENTS
  Sam Okafor (PM)
  └─ Claude: synthesise discovery interview notes → jobs-to-be-done
  └─ Claude: draft PRD with problem statement, success metrics, ACs (Given/When/Then)
      │
      ▼
  Chris Wang (BA) [for API/integration features]
  └─ Claude: write functional requirements (REQ-API2-NNN) from Sam's PRD
  └─ Claude: document process flows + data mappings
      │
      ▼
ENGINEERING
  Alex Rivera (Dev)
  └─ Claude: code review, debug, unit test generation, PR descriptions
  └─ Claude: DB migration review before raising for Staff Engineer sign-off
      │
      ▼
QA
  Jordan Lee (QA)
  └─ Claude: generate Cypress E2E tests from Sam's ACs
  └─ Claude: write manual test plan in TestRail format
  └─ Claude: triage flaky tests, write bug reports
      │
      ▼
RELEASE
  Sam Okafor (PM)
  └─ Claude: draft release notes (benefit-first, no jargon)
  └─ Claude: write sprint retrospective summary
      │
      ▼
POST-RELEASE
  Morgan Blake (Ops)
  └─ Claude: update runbooks for new error codes/flows introduced
  └─ Claude: draft post-mortem for any incidents triggered by the release
      │
      ▼
  Priya Nair (Data)
  └─ Claude: build/update dbt models to track new feature metrics
  └─ Claude: update Looker dashboards for Sam's OKR tracking
```

---

## Shared Handoff Points

### Alex → Jordan (Code → QA)
- Alex notifies Jordan when `job_assignment` or `status-machine` code changes
- Jordan must sign off before Alex merges any such change
- **Claude's role:** Alex uses Claude to generate unit tests; Jordan uses Claude to generate Cypress E2E tests from the same ACs

### Sam → Alex (Requirements → Engineering)
- Sam provides Given/When/Then ACs in Jira
- Alex uses ACs as input to Claude for test generation and implementation guidance
- **Shared rule:** No ship date committed without Alex confirming feasibility first

### Alex → Morgan (Shipping → Ops)
- Alex notifies Morgan before any change to error codes or log formats
- Morgan uses this to update Datadog alert rules and runbooks before the deploy
- **Claude's role:** Morgan uses Claude to draft updated runbook steps from Alex's PR descriptions

### Alex → Priya (Events → Data)
- Alex notifies Priya 1 sprint ahead of any dispatch_events schema change
- Priya uses the heads-up to update affected dbt staging models before the schema change lands
- **Claude's role:** Priya uses Claude to identify downstream model impact from a schema diff

### Chris → Alex (Requirements → Implementation)
- Chris hands off approved functional requirements (REQ-API2-NNN) to Alex for API v2 implementation
- Alex flags feasibility issues back to Chris; Chris updates the spec
- **Claude's role:** Chris uses Claude to write requirements; Alex uses Claude to generate endpoint stubs from them

### Morgan → Priya (Incidents → Data)
- Morgan uses Priya's Looker SLA dashboard every Monday for incident review
- Morgan flags data anomalies back to Priya (e.g., incorrect breach counts from cancelled jobs)
- **Claude's role:** Priya uses Claude to investigate metric discrepancies flagged by Morgan

---

## Cross-Persona Claude Patterns

### Pattern 1 — Requirements → Tests
Sam writes ACs → Jordan pastes them directly into the QA test generation prompt.
No translation step needed if ACs are in Given/When/Then format.

### Pattern 2 — Incident → Runbook → Model
Morgan identifies a gap in incident response → uses Claude to write the runbook →
flags the metric gap to Priya → Priya uses Claude to add a dbt model or Datadog monitor.

### Pattern 3 — PR Description → Release Notes
Alex writes a PR description with Claude (business reason first) → Sam uses the PR
description as input to Claude to draft release notes (strips technical detail,
adds benefit language).

### Pattern 4 — Bug Report → AC Update
Jordan files a bug report → Sam uses Claude to check whether the bug represents a
missing AC → if yes, Sam updates the spec with a new Given/When/Then → Jordan
generates a regression test from the new AC.

---

## Overall Work Patterns

### What Claude Does Well Across All Personas
- Translating between audiences (technical ↔ non-technical)
- Following established conventions when given the right context
- Generating first drafts that are 80% right — faster than starting from blank
- Catching gaps (missing error paths, missing tests, missing rollback scripts)

### What Claude Should Not Do Autonomously (Any Persona)
- Make product decisions (that's Sam's job)
- Commit to ship dates or scope changes
- Publish, send, or deploy anything without human review
- Access production systems with write permissions
- Handle security incidents through normal channels

### How to Give Claude Good Context
Every persona's prompt should include:
1. **Who you are** — role, squad, company context (or just load your CLAUDE.md)
2. **What you need** — specific artefact, not just "help me with X"
3. **The constraints** — conventions, tools, who else is affected
4. **The input** — the actual code, notes, or data to work with

### Iteration Principle
Claude's first output is a draft. Use your `eval_checklist.md` to score it.
If it scores below threshold: identify the specific gap, add that constraint to
your prompt, and re-run. Track what worked in your `eval_checklist.md` iteration log.

---

## Useful Cross-Persona Prompt Chains

### Chain 1: Feature from Discovery to Tests
```
Sam:    "Synthesise these interview notes into a PRD"
         ↓ (paste PRD ACs into)
Jordan: "Write Cypress E2E tests for these Given/When/Then ACs"
```

### Chain 2: Incident to Runbook to Dashboard
```
Morgan: "Write a post-mortem from this incident timeline"
         ↓ (paste root cause into)
Morgan: "Write a runbook for this failure mode"
         ↓ (paste runbook symptoms into)
Priya:  "Write a Datadog monitor for this metric condition"
```

### Chain 3: PR to Release Notes
```
Alex:   "Write a PR description for this change"
         ↓ (paste PR description into)
Sam:    "Write release notes from this PR description for a non-technical audience"
```
