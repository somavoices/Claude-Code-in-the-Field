# 05 — Safety and Risk
## When to Trust Claude, When to Verify, and What Can Go Wrong

---

## What You Will Learn

How Claude can fail, what the real risks of AI tools at work are, how to evaluate
output before using it, and why the safety guardrails in this kit exist.

**Estimated reading time: 20 minutes.**

---

## The Fundamental Risk: Claude Sounds Confident When It Is Wrong

This is the most important thing to understand about Claude.

Claude does not say "I'm not sure" very often. It produces fluent, confident,
well-structured responses — even when those responses are incorrect. This is
called **hallucination**: Claude generates text that sounds correct but is not.

**Examples of hallucination in practice:**

- Claude writes a SQL query that looks correct but uses the wrong column name
  (one that does not exist in your actual schema)
- Claude generates a Cypress test using a selector pattern that is plausible
  but not the one your codebase actually uses
- Claude recommends a Node.js library method that was deprecated in a version
  you are running
- Claude writes functional requirements that are technically correct but
  contradict a business rule documented in your memory.md
- Claude produces a runbook step that describes a command that works in some
  environments but not yours

**The rule:** Claude's confidence is not a reliable indicator of correctness.
Always verify outputs against your actual system before using them.

---

## The Five Categories of Risk

### 1. Technical Incorrectness
Code that compiles but does not work correctly. SQL that runs but returns
wrong results. Tests that pass but do not actually verify the right behaviour.

**How to catch it:** Run the code. Test the SQL on staging data. Execute the tests.
Do not assume correct syntax means correct behaviour.

### 2. Security Vulnerabilities
Claude can generate code with SQL injection, hardcoded credentials, missing
authentication, overly permissive file access, or other security flaws — especially
if you ask for a "quick example" without specifying security requirements.

**How to catch it:** The developer safety_checklist.md covers the most common
patterns. For any code that handles authentication, payments, or user data,
review with your security team before deploying.

### 3. Data Privacy (PII)
If you paste customer data, employee data, or any personally identifiable
information into a Claude prompt, that data is sent to Anthropic's API.

**The rule:** Never paste real customer names, email addresses, account IDs,
or any personally identifiable information into Claude prompts. Use anonymised
or synthetic data in examples.

**Each persona's safety_checklist.md includes a specific check for this.**

### 4. Confidentiality
Intellectual property, unpublished product plans, unreleased financial figures,
and proprietary system architecture are all confidential. Pasting them into
Claude sends them to an external API.

**The rule:** Check your organisation's AI usage policy before sharing sensitive
internal information with Claude. When in doubt, anonymise or abstract the details
before pasting.

### 5. Irreversible Actions
Some actions cannot be undone: pushing to a production branch, sending a Zendesk
reply to a customer, deleting a database record, running a DROP TABLE. If Claude
suggests one of these and you act on it without review, there may be no way to
reverse the consequences.

**The rule:** This is why settings.json permissions exist. They prevent Claude
from even suggesting these actions in many cases. And they remind you — via
escalation triggers — when a human decision is required.

---

## What Hallucination Looks Like in Each Role

### Developer
Claude generates a function that calls `getJobById(id)` — but your codebase
has `findJobById(id)`. The code looks correct but will fail at runtime.

### Data & Analytics
Claude joins `dispatch_jobs` to `dim_customer` on `customer_id` — but in your
schema it should be `account_id`. The query runs but returns wrong results silently.

### QA & Testing
Claude writes `cy.get('[data-testid="assign-btn"]')` — but the actual testid in
your component is `data-testid="technician-assign-button"`. The test runs but
always fails, and it is not obvious why.

### Product Manager
Claude writes an acceptance criterion that says "notify the dispatcher" — but
your product only notifies technicians, not dispatchers. Sam's team builds it,
ships it, and the customer finds the wrong behaviour.

### Business Analyst
Claude writes `REQ-API2-007: The system shall return a 404 when a job is not found`
— but your API convention is to return 200 with an empty object. The requirement
is internally consistent but wrong for your system.

### Ops & Support
Claude writes a runbook step: `restart the dispatch consumer service using
systemctl restart dispatch-consumer` — but your deployment uses AWS ECS, not
systemctl. The step will fail during an active P1 incident.

---

## The Mitigation Framework: Three Lines of Defence

### Line 1 — Good Prompts Reduce Hallucination
When you give Claude your actual schemas, your actual conventions, and your
actual constraints, it is much less likely to invent things. Loading your
CLAUDE.md, memory.md, and SKILL.md dramatically reduces hallucination because
Claude is matching against your real context rather than guessing.

**Good prompting is your first defence.**

### Line 2 — Safety Checklist Catches Issues Before You Act
Your persona's `safety_checklist.md` is your second defence. Before you use
any Claude output, run through the relevant section. The checks are calibrated
to the most common failure modes for your role.

**The checklist is your second defence.**

### Line 3 — Testing and Human Review Catches What the Checklist Misses
Run the code. Test the SQL on staging. Execute the Cypress tests. Have a colleague
read the requirements. Verify the runbook step in a test environment.

No amount of good prompting eliminates the need for human verification on
anything that goes to production.

**Human verification is your third defence.**

---

## When to Trust Claude's Output

**Higher trust (still verify, but less critical):**
- Draft documents (PRDs, release notes, post-mortems) — easy for a human to spot errors
- Code boilerplate (test scaffolding, config files) — easy to verify by reading
- Summaries of information you provided — Claude is summarising your own input
- Explanations of concepts — easier to fact-check

**Lower trust (verify carefully before using):**
- SQL queries against your actual database — run on staging first
- Code that touches security, auth, or payments — security review required
- Requirements or ACs that will drive engineering work — domain expert review
- Runbook steps that will be executed during a live incident — test in dry run first
- Any output that will be sent to customers — human review mandatory

**Never trust without review:**
- Any output suggesting a destructive database operation
- Any code that handles credentials, secrets, or tokens
- Any customer-facing communication drafted by Claude
- Any commit to a production branch suggested by Claude

---

## How the Safety Guardrails in This Kit Help

### settings.json permissions
Prevent Claude from even attempting operations outside the defined boundaries.
For example, the developer's settings deny DELETE/DROP/TRUNCATE — Claude will
not suggest running these without a second engineer approval trigger.

### settings.json hooks
Automatically block dangerous patterns before they execute. The developer's
PreToolUse hook intercepts any Bash command containing DROP or TRUNCATE and
stops it with an explicit error message. This catches errors even when a human
might overlook them.

### safety_checklist.md
A structured human review step — applied after seeing Claude's output, before
acting on it. Each persona's checklist is tuned to that role's most common risks.

### eval_checklist.md
Goes further than safety — scores output quality and tells you whether to
use it as-is, revise it, or reject it entirely and re-prompt.

---

## The Non-Negotiable Rules (All Roles)

Regardless of your role, these apply in every session:

1. **Never paste real customer data into Claude** — use anonymised examples
2. **Never paste credentials, API keys, or passwords** — use placeholder values
3. **Never act on Claude output that affects production without human review**
4. **Never let Claude send, publish, or deploy anything autonomously**
5. **When in doubt, verify** — Claude's confidence is not a guarantee of correctness

---

## Your Next Step

Go to [06_hooks_and_automation.md](06_hooks_and_automation.md) to learn how
hooks work and how they automatically enforce the safety rules you just read about.
