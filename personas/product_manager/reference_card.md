# Reference Card — Sam Okafor, Product Manager

## Prompt Engineering Best Practices

### The 4-Part Prompt Structure
```
[CONTEXT]   Audience + product area (Dispatch Core) + business situation
[TASK]      What document/output you need — PRD, ACs, update, release notes
[RULES]     No ship date commitments, no unreleased features, audience-appropriate language
[INPUT]     Feature description, customer pain, interview notes, or sprint list
```

### Make Prompts Specific to Your Context
| Vague | Specific |
|---|---|
| "Write acceptance criteria" | "Write Given/When/Then ACs for dispatcher re-routing a job mid-shift — cover success, unavailable technician, and permission boundary (dispatcher vs. technician)" |
| "Write a status update" | "Write a 3-bullet Amber status update for Rachel Kim on Project Falcon — ship date at risk, root cause is Mobile squad dependency" |
| "Write release notes" | "Write release notes for dispatchers and ops managers — benefit-first, no Jira references, no roadmap hints" |

### Context Shortcuts
- **Product:** "Field service management platform — dispatchers assign technicians to jobs in real time"
- **Squad:** "Dispatch Core — Alex Rivera (Dev), Jordan Lee (QA), ship date May 30 for Project Falcon"
- **Rules:** "No date commitments without Alex confirmation, no unreleased features, PM review before publish"

---

## Claude for Daily PM Work

| Task | Best Prompt Type | Key Rule |
|---|---|---|
| PRD | Describe customer pain + goals | Problem before solution |
| Acceptance criteria | Paste user story | Given/When/Then, include error path |
| Status update | Describe current state + risk | RAG status + one next action |
| Discovery synthesis | Paste interview notes | Remove PII, extract jobs-to-be-done |
| Release notes | List sprint changes | Benefit-first, no jargon |
| Sprint brief | List tickets + capacity | Include definition of done reminder |

---

## Iteration Patterns

- **ACs missing error path** → "Add a Given/When/Then for when the technician is unavailable"
- **PRD too solution-focused** → "Rewrite the problem statement — describe the customer pain, not the feature"
- **Update too long** → "Shorten to: one headline, RAG status, 3 bullets, one next action"
- **Release notes have jargon** → "Rewrite for a non-technical operations manager — no API terms"

---

## Safety Reminders

- Never let Claude commit to a ship date — always "TBD pending engineering input"
- Never reference features marked "under consideration" as confirmed in any output
- All external-facing content (release notes, API docs) needs your review before use
- Never include customer names or PII in any prompt or output
