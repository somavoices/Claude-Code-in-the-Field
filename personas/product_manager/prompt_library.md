# Prompt Library — Sam Okafor, Product Manager

## How to Use This File

**Do not load this file into Claude.** This is a reference for you, not context for Claude.

1. Find the prompt that matches your task
2. Copy it into your Claude Code session
3. Replace the `[PLACEHOLDER]` values with your actual input
4. Run it — Claude already has your context from `CLAUDE.md` and `memory.md`

Adapt prompts over time. If a prompt consistently needs the same tweak, edit this file.

---

## 1. Write a PRD

```
Write a product requirements document (PRD) for the following feature on the
field service management platform (B2B SaaS, ~300 employees, Dispatch Core squad).

Structure:
1. Problem Statement (customer pain, not solution)
2. Success Metrics (current → target, with dates)
3. Scope (in scope + explicit out of scope)
4. User Stories with Acceptance Criteria in Given/When/Then format
5. Edge Cases & Constraints
6. Dependencies (note: Mobile squad needs 2-week heads-up for API changes)
7. Open Questions table

Rules:
- Every AC must include an error path, not just the happy path
- Include permission boundaries: dispatcher vs. technician vs. admin
- Include empty state behaviour
- Do not commit to a ship date — leave as TBD pending engineering input
- Do not reference any feature not already announced externally

Feature to document: [DESCRIBE THE FEATURE AND THE CUSTOMER PROBLEM IT SOLVES]
Jira epic: FSM-[XXXX]
```

---

## 2. Write Acceptance Criteria

```
Write acceptance criteria for the following user story on the field service management platform.

Use Given/When/Then format. For each AC:
- Cover the happy path
- Cover at least one error or failure path
- Cover the permission boundary (which role can do this — dispatcher/technician/admin)
- Cover the empty state if applicable

Avoid vague language like "should notify" — be specific about channel, timing, and content.

User story: [PASTE USER STORY]
Known constraints: [ANY TECHNICAL CONSTRAINTS ALEX FLAGGED]
```

---

## 3. Stakeholder Status Update

```
Write a stakeholder status update for Project Falcon (FSM-1201) — Real-Time Dispatch Redesign.

Audience: [Rachel Kim (VP Product) / executive team / TechServ Group (customer)]
Format: Headline → RAG status (Red/Amber/Green) → 3 bullet points max → Next action

Context:
- Ship date: May 30 (hard — TechServ Group contract commitment)
- Current status: [DESCRIBE CURRENT STATE]
- Risks: [ANY OPEN RISKS]
- Blockers: [ANY BLOCKERS]

Rules:
- Lead with customer impact, not technical detail
- Do not commit to scope changes or new dates in the update
- Do not reference internal team names or org structure in customer-facing version
- Flag any Red status to Rachel Kim before sending
```

---

## 4. Discovery Interview Synthesis

```
Synthesise the following customer discovery interview notes into a structured summary
for the Dispatch Core product backlog.

Output format:
1. Top 3 pain points (ranked by frequency/severity)
2. Verbatim quotes (2–3 per pain point — do not paraphrase)
3. Jobs to be done (what the customer is trying to accomplish)
4. Signals for roadmap (what they asked for vs. what they actually need)
5. Recommended next step (backlog item or open question)

Rules:
- Do not include customer name, company name, or any identifying information
- Flag any mention of a competitor product or feature comparison
- If the customer referenced a specific timeline expectation, call it out separately

Interview notes:
[PASTE NOTES HERE]
```

---

## 5. Release Notes

```
Write release notes for the following product update on the field service management platform.

Audience: Enterprise customers (dispatchers and operations managers — non-technical)
Tone: Professional, benefit-first, plain language. No engineering jargon.
Format: one-paragraph summary + bulleted list of changes

Rules:
- Lead with what customers can now do, not what we built
- Do not reference internal Jira ticket numbers
- Do not reference any future features or roadmap items
- Do not include any pricing information
- These are draft only — Sam Okafor will review before publish

Changes in this release:
[LIST OF CHANGES FROM SPRINT]
```

---

## 6. Sprint Planning Brief

```
Write a sprint planning brief for the Dispatch Core squad for the upcoming sprint.

Context:
- Squad: Alex Rivera (FS Engineer), Jordan Lee (QA), + 3 engineers, 1 designer
- Sprint duration: 2 weeks
- Active epic: Project Falcon (FSM-1201), target ship May 30

Include:
- Sprint goal (one sentence — what does success look like at the end of this sprint?)
- Committed stories (from backlog — I will provide list)
- Capacity risks (who is out, any known dependencies on other squads)
- Definition of done reminder: Jordan's test sign-off required, SonarQube passing

Backlog items to include:
[LIST JIRA TICKETS AND ESTIMATES]
Capacity notes: [ANY ABSENCES OR CROSS-TEAM COMMITMENTS]
```
