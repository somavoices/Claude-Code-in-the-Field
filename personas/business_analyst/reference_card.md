# Reference Card — Chris Wang, Business Analyst

## How to Use This File

**Do not load this file into Claude.** Keep it open alongside your Claude Code session.

- Before writing a prompt: check the 4-part structure and context shortcuts
- When a prompt produces weak output: use the vague vs. specific examples to diagnose why
- When iterating: use the iteration patterns section to identify what to fix

---

## Prompt Engineering Best Practices

### The 4-Part Prompt Structure
```
[CONTEXT]   Integration type + squad context (Integrations/API v2) + source of requirement
[TASK]      What spec artefact you need — requirements, flow, data mapping, gap analysis
[RULES]     shall/should language, verifiable, no PII, no product decisions
[INPUT]     Customer notes, existing API spec, interview transcript, or Zendesk history
```

### Make Prompts Specific to Your Context
| Vague | Specific |
|---|---|
| "Write requirements" | "Write functional requirements (REQ-API2-NNN) for the webhook delivery endpoint — cover happy path, 401, 429, 500, and retry behaviour by customer tier" |
| "Document this flow" | "Write a swim-lane process flow for webhook delivery — actors: customer system, FSM API, retry queue. Include success path and DLQ exception path" |
| "Map these fields" | "Write a data mapping table from the v1 job object to the v2 webhook payload — flag any type changes and new required fields" |

### Context Shortcuts
- **Product:** "Field service management API — job statuses: pending, assigned, en_route, on_site, complete, cancelled"
- **API rules:** "Bearer token auth, ISO 8601 UTC timestamps, rate limits by tier (Enterprise 1000/min, Business 200/min, Starter 50/min)"
- **Conventions:** "shall = mandatory, should = recommended, REQ-API2-NNN IDs, every req traces to a source"

---

## Claude for Daily BA Work

| Task | Best Prompt Type | Key Rule |
|---|---|---|
| Functional requirements | Describe feature + source | shall/should, verifiable, with error handling |
| Process flow | Describe actors + workflow | Include all exception paths |
| Data mapping | List source + target fields | Explicit transformation rule for each field |
| Gap analysis | Paste v1 spec + v2 requirements | Note migration impact per customer |
| Requirements review | Paste requirements doc | Check verifiability + traceability |
| Discovery synthesis | Paste customer notes | Remove PII, flag product decisions |

---

## Iteration Patterns

- **Requirement not verifiable** → "Rewrite REQ-API2-NNN so a tester can write a test case from it"
- **Missing error handling** → "Add requirements for: missing auth token (401), rate limit exceeded (429), malformed body (400)"
- **Flow ends at 'error occurs'** → "Add the exception path — what happens after the error, who is notified, what is the end state"
- **Requirement says 'how'** → "Remove implementation detail — specify behaviour only"

---

## Safety Reminders

- Never include customer company names or contact details in requirements documents
- Never make product decisions — document the question and flag it to Sam Okafor
- Never publish to Confluence without your own review first
- If a requirement touches auth or rate limiting: confirm with Alex Rivera before finalising
