# 04 — Your First Prompt
## A Hands-On Walkthrough for Every Role

---

## What You Will Do

Write a prompt, run it, evaluate the output, identify what is wrong,
improve the prompt, and run it again. By the end you will have completed
one full iteration cycle — the core skill for effective Claude use.

**Estimated time: 30 minutes.**

---

## Before You Start

Make sure you have completed:
- [x] Claude Code installed and authenticated (01_installation_setup.md)
- [x] You understand what context is and why it matters (02_core_concepts.md)

Open a Claude Code session and navigate to the training kit folder.

---

## Part 1 — The Weak Prompt (Do This First)

Every role has a different first task. Find yours below and run the weak prompt
exactly as written — do not add anything to it.

### Developer (Alex Rivera)
```
Review this code for bugs.

function assignTechnician(jobId, technicianId) {
  const job = db.query(`SELECT * FROM dispatch_jobs WHERE id = ${jobId}`);
  if (job.status === 'pending') {
    db.query(`UPDATE dispatch_jobs SET technician_id = ${technicianId}, status = 'assigned'`);
    console.log('Assigned');
  }
}
```

### Data & Analytics (Priya Nair)
```
Write a SQL query to count SLA breaches by customer.
```

### QA & Testing (Jordan Lee)
```
Write a test for the login page.
```

### Product Manager (Sam Okafor)
```
Write acceptance criteria for a job assignment feature.
```

### Business Analyst (Chris Wang)
```
Write requirements for an API endpoint.
```

### Ops & Support (Morgan Blake)
```
Write a runbook for when the service goes down.
```

---

## Part 2 — Observe What You Got

Read Claude's response carefully. It probably produced something — but notice:

**Questions to ask yourself:**
1. Does this output reflect *your team's* conventions — or is it completely generic?
2. Does Claude know what tech stack you use?
3. Does the output reference your actual domain (field service management, dispatching, technicians)?
4. Would you be able to use this output directly, or does it need significant rework?
5. Are there things missing that you would expect a colleague to catch?

**What you likely found:**
- Developer: Claude caught the SQL injection but may have missed the `console.log`, the missing lock on concurrent calls, or the `SELECT *`
- Data: Claude wrote SQL but with made-up table names, no tier-based SLA thresholds, and no CTE structure
- QA: Claude wrote a test but with wrong selector strategy, possibly wrong test framework, no real data isolation
- PM: Claude wrote ACs but in a generic format, not Given/When/Then, missing error paths
- BA: Claude wrote requirements but with "must" instead of "shall", no REQ IDs, no error codes
- Ops: Claude wrote runbook steps but vague ("check the logs", "contact engineering") with no Datadog queries

**This is the weak prompt problem.** Claude is doing its best with no context.

---

## Part 3 — Load Your Context Files

Now load the context files that tell Claude who you are and what your standards are.

In your Claude Code session, type:

```
Please read the following files and use them as your context for this session:

1. CLAUDE.md (root — shared company context)
2. personas/[your-role]/CLAUDE.md
3. personas/[your-role]/memory.md
4. personas/[your-role]/SKILL.md

Confirm when you have read all four files and tell me: who are you,
what are you currently working on, and what are the most important
conventions I should know about?
```

Replace `[your-role]` with your folder name:
`developer` / `data_analytics` / `qa_testing` / `product_manager` / `business_analyst` / `ops_support`

**Claude should respond with something like:**

*"I am Alex Rivera, Full Stack Engineer on the Dispatch Core squad at a mid-size B2B SaaS
company building a field service management platform. I am currently working on Project
Falcon — rebuilding the dispatch engine with RabbitMQ. Key conventions: TypeScript strict
mode, no console.log (use logger), monetary values as integers, CTEs over subqueries..."*

If Claude's response reflects your persona correctly, the context is loaded.

---

## Part 4 — The Strong Prompt

Now run the improved version of the same task. Your persona's `prompt_library.md`
has a complete version — but first, try building your own using the **4-part structure**:

```
[CONTEXT]   One sentence about who you are and what system this is for
[TASK]      Exactly what you want Claude to produce
[RULES]     Your team's specific standards and what to look for / avoid
[INPUT]     The actual code, question, or data to work with
```

### Developer example (strong prompt):
```
[CONTEXT]
I am a Full Stack Engineer on the Dispatch Core squad. This is a Node.js/TypeScript
service using PostgreSQL. We use a query builder — never raw db.query() calls.

[TASK]
Review this function for bugs and code quality issues.

[RULES]
Check specifically for:
- SQL injection vulnerabilities
- Raw db.query() calls that bypass the query builder
- Missing error handling on async operations
- console.log usage (must use logger from '@/lib/logger')
- Missing locking for concurrent dispatch operations
- SELECT * usage (always use explicit column lists)

For each issue: state the line, explain the risk, provide the fix.

[INPUT]
function assignTechnician(jobId, technicianId) {
  const job = db.query(`SELECT * FROM dispatch_jobs WHERE id = ${jobId}`);
  if (job.status === 'pending') {
    db.query(`UPDATE dispatch_jobs SET technician_id = ${technicianId}, status = 'assigned'`);
    console.log('Assigned');
  }
}
```

### Data & Analytics example (strong prompt):
```
[CONTEXT]
I am a Senior Data Analyst. Our stack: Snowflake, dbt, field service management platform.
SLA thresholds by tier: Enterprise = 4 hours, Business = 8 hours, Starter = 24 hours.
SLA clock: starts at job.scheduled_at, stops at job.completed_at. Exclude cancelled jobs.

[TASK]
Write a SQL query that counts SLA breaches per customer tier for the last 30 days.

[RULES]
- Use CTEs, not subqueries
- Explicit column lists — no SELECT *
- All timestamps in UTC
- Filter out cancelled jobs (status = 'cancelled')
- Group by customer tier, not customer name

[INPUT]
Tables available:
- dispatch_jobs (job_id, customer_id, status, scheduled_at, completed_at)
- dim_customer (customer_id, tier) — tier values: 'Enterprise', 'Business', 'Starter'
```

Run your strong prompt now.

---

## Part 5 — Compare the Outputs

Read both responses. Fill in this quick comparison:

| | Weak prompt output | Strong prompt output |
|---|---|---|
| Does it match your tech stack? | | |
| Does it use your team's conventions? | | |
| Is the output usable without heavy editing? | | |
| Did it catch all the important issues/requirements? | | |
| Would a colleague be satisfied with this? | | |

**The difference you should see:**
- The strong prompt output references your actual stack, your actual domain, your actual rules
- Issues are specific (line numbers, field names, exact error codes)
- The output format matches what your team uses
- You could use it with minor edits, not a full rewrite

---

## Part 6 — The Iteration Cycle

Even a strong prompt does not always produce perfect output the first time.
When something is wrong, do not start over — iterate.

**The three-step iteration process:**

1. **Identify the specific gap** — not "this is wrong" but "the error path is missing" or "it uses SELECT * in the mart model" or "the escalation section has no time threshold"

2. **Add that specific constraint to your prompt** — one targeted addition is better than rewriting the whole prompt

3. **Re-run** — Claude will correct the specific gap while keeping what was right

**Example:**
```
First run: Good code review but missed the missing lock for concurrent dispatch.

Your follow-up:
> The review missed one issue: this function can be called concurrently by two
> dispatchers assigning the same technician. There is no distributed lock.
> We use a Redis distributed lock — see src/lib/locks/dispatch.lock.ts.
> Add a finding for this with the specific risk and how to fix it.
```

Claude adds the missing finding without redoing the whole review.

---

## Part 7 — What You Just Learned

You completed the fundamental Claude Code workflow:

```
Load context → Write a specific prompt → Evaluate output →
Identify the gap → Add it to the prompt → Re-run → Better output
```

This cycle is the core skill. Everything else in the training kit — the safety
checklist, the eval checklist, the agent instructions — is built on top of this loop.

**Key takeaways:**
- Context transforms generic output into role-specific, team-specific output
- The 4-part prompt structure (Context / Task / Rules / Input) is the template
- Iteration is normal — expect to run 2–3 times for complex tasks
- The prompt_library.md in your persona folder has ready-made prompts so you do not
  have to build from scratch every time

---

## Your Next Step

Go to [05_safety_and_risk.md](05_safety_and_risk.md) to learn how to evaluate
Claude's output critically and understand what can go wrong when using AI at work.
