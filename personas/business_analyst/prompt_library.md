# Prompt Library — Chris Wang, Business Analyst

---

## 1. Write Functional Requirements

```
Write functional requirements for the following API endpoint or integration feature
on the field service management platform (B2B SaaS, Integrations squad).

Format:
- Requirement IDs: REQ-API2-[NNN] format, sequential
- Use "shall" for mandatory, "should" for recommended
- Every requirement must be verifiable (a tester can write a test from it)
- Every requirement traces to a source (Jira ticket or product decision)
- Include: happy path requirements, error handling (401, 429, 400, 500), data mapping table
- Include open questions table with owner and due date

Rules:
- Do not specify implementation — specify behaviour only
- Do not include customer names or PII
- All timestamps: ISO 8601 UTC
- Rate limits by tier: Enterprise 1000 req/min, Business 200 req/min, Starter 50 req/min
- Auth: Bearer token on all endpoints

Feature to document: [DESCRIBE THE ENDPOINT OR INTEGRATION FLOW]
Jira epic: FSM-[XXXX]
Source: [CUSTOMER REQUEST / PRODUCT DECISION BY SAM OKAFOR]
```

---

## 2. Process Flow Documentation

```
Write a process flow description for the following integration workflow on the
field service management platform.

Output a swim-lane flow with:
- Actors (customer system, FSM API, Dispatch Core, Notifications)
- Happy path: numbered steps in sequence
- Exception paths: labelled branches for each failure condition
- Decision points: Yes/No branches with conditions stated
- End states: what is the final state for each path (success, failure, retry)

Rules:
- Label every exception path — do not end at "error occurs"
- State which system owns each step
- If a step has a timeout or retry, specify it
- Note where Morgan Blake (Ops) needs a runbook entry

Workflow to document: [DESCRIBE THE INTEGRATION FLOW]
```

---

## 3. Gap Analysis

```
Perform a gap analysis between the current API v1 behaviour and the proposed API v2
requirements for the field service management platform.

Context:
- API v1 must remain live for 12 months after v2 GA
- v2 scope: REST versioning (/api/v2/), webhooks for job status changes, rate limiting
- Out of scope: GraphQL, streaming, OAuth2
- TechServ Group polls /api/v1/jobs every 60 seconds — migration path must be documented

For each gap, produce:
- Gap ID (GAP-[NNN])
- Description: what exists in v1 vs. what v2 requires
- Impact: which customers/integrations are affected
- Migration action required: yes/no, and what
- Owner: BA (document), Engineering (implement), PM (decide)

v1 behaviour: [DESCRIBE OR PASTE v1 API SPEC EXCERPT]
v2 requirement: [DESCRIBE OR PASTE RELEVANT REQ-API2 REQUIREMENTS]
```

---

## 4. Data Mapping Table

```
Write a data mapping table for the following integration between the field service
management platform and an external system.

Table format:
| Source Field | Source Type | Transformation Rule | Target Field | Target Type | Required | Notes |

Rules:
- Transformation rule must be explicit — "copy" is not enough if there's a type cast or format change
- All timestamps: source may vary, target is always ISO 8601 UTC
- Flag any field where the mapping is ambiguous or needs Alex Rivera confirmation
- Flag any PII fields — do not include example values containing real customer data
- Note if a field is new in v2 (not in v1)

Source system: [SOURCE]
Target system: [TARGET]
Fields to map: [LIST OR DESCRIBE]
```

---

## 5. Requirements Review Checklist

```
Review the following functional requirements document for the field service management
platform (Integrations squad) before Sam Okafor sign-off.

Check for:
- Every requirement uses "shall" or "should" (not "must", "will", "needs to")
- Every requirement is verifiable (can a tester write a test from it?)
- Every requirement traces to a source
- Error handling documented: 401, 429, 400, 500 minimum for each endpoint
- All timestamps specified as ISO 8601 UTC
- Rate limits specified per customer tier
- Authentication requirement stated (Bearer token)
- Open questions have owner and due date
- No customer names or PII in the document
- No implementation detail (only behaviour)

Requirements document:
[PASTE REQUIREMENTS HERE]
```

---

## 6. Stakeholder Requirements Summary

```
Summarise the following customer discovery notes or support tickets into structured
requirements input for the Integrations squad.

Output:
1. Stated needs (what the customer explicitly asked for)
2. Implied needs (what they need but didn't say — inferred from workflow description)
3. Constraints (what they cannot change on their side)
4. Questions to clarify before writing requirements (with suggested owner)
5. Suggested requirement areas (not full requirements — input for the spec)

Rules:
- Remove all customer identifying information from the output
- Flag any request that is out of scope for API v2 (GraphQL, streaming, OAuth2)
- Flag any request that implies a product decision — refer to Sam Okafor
- Do not write full requirements here — this is input, not output

Source material:
[PASTE CUSTOMER NOTES / ZENDESK TICKETS / INTERVIEW TRANSCRIPT]
```
