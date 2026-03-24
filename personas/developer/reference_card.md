# Reference Card — Alex Rivera, Full Stack Engineer

## Prompt Engineering Best Practices

### The 4-Part Prompt Structure
```
[CONTEXT]   Who you are + what system/codebase is involved
[TASK]      What you want Claude to do — specific, verifiable
[RULES]     Constraints: language, style, what to avoid
[INPUT]     The actual code, diff, error, or question
```

### Make Prompts Specific to Your Stack
| Vague | Specific |
|---|---|
| "Review this code" | "Review this TypeScript function for N+1 queries, missing error handling, and strict mode violations" |
| "Write a test" | "Write Jest unit tests covering happy path, null input, and DB error — mock the query builder" |
| "Fix this bug" | "This dispatch assign endpoint returns 200 but doesn't update the DB — here's the code and the logs" |

### Context Shortcuts (use these in every prompt)
- **Stack:** "Node.js/TypeScript strict, PostgreSQL, Jest, SonarQube 80% gate"
- **Conventions:** "CTEs over subqueries, named exports, co-located tests, logger not console.log"
- **Squad:** "Dispatch Core — changes may affect Mobile squad and Priya's Data models"

---

## Claude for Daily Engineering Work

| Task | Best Prompt Type | Key Rule |
|---|---|---|
| PR review | Paste full diff + specific checks | List exactly what to look for |
| Debug | Paste logs + code + symptom | State what changed before the bug |
| Unit tests | Paste function + requirements | Specify mock strategy |
| SQL query | Paste schema + requirement | State table sizes if >1M rows |
| PR description | Describe change + Jira ticket | Mention downstream impact |
| DB migration | Paste SQL + table sizes | Always ask for rollback script |

---

## Iteration Patterns

- **Claude gives wrong code style** → Add your conventions to the prompt explicitly
- **Claude suggests unsafe change** → Remind it of the constraint ("never DROP without WHERE")
- **Claude's test doesn't cover edge case** → "Add tests for: null input, empty array, concurrent call"
- **Answer is too long** → "Answer in bullet points, code only, no explanation"
- **Answer is too shallow** → "Go deeper on X — explain the risk and the fix"

---

## Safety Reminders

- Never paste `.env` values or secrets into prompts
- Never ask Claude to push to `main` or approve a production deploy
- If Claude suggests disabling SonarQube: reject — fix the underlying issue instead
- Review all generated SQL for missing WHERE clauses before running
