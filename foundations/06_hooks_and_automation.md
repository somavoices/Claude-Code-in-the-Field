# 06 — Hooks and Automation
## Making Safety Automatic With Claude Code Hooks

---

## What You Will Learn

What hooks are, how they work, how to read the hooks in your persona's settings.json,
and how they protect you from common mistakes.

---

## The Problem Hooks Solve

In the previous document you learned that you should always verify Claude's output
before acting on it. But humans are fallible — especially under deadline pressure.
A developer rushing to fix a bug before a release might overlook a missing WHERE
clause. A QA engineer under pressure might not notice a hardcoded `cy.wait(3000)`.
An ops engineer responding to a P1 at 2am might not notice a write command in a
runbook step.

Hooks solve this by moving safety checks out of the human's head and into the system.
They run automatically — before you can even see the result of a dangerous action.

---

## What Is a Hook?

A hook is a shell command that Claude Code runs automatically at a specific moment
in its workflow. You configure hooks in your `settings.json` file and they run
every session without you having to think about them.

Think of hooks like the safety checks built into modern power tools: the blade
guard on a table saw does not ask you if you want to cut safely — it just stops
the blade if your hand gets too close. Hooks work the same way.

---

## The Three Hook Types

### PreToolUse Hook
Runs **before** Claude executes a tool action. This is the most important type —
it can block the action entirely if it detects a dangerous pattern.

**Example in the developer's settings.json:**
```json
{
  "matcher": "Bash",
  "hooks": [{
    "type": "command",
    "command": "if echo \"$CLAUDE_TOOL_INPUT\" | grep -qiE '(DROP|TRUNCATE|DELETE FROM)';
                then echo 'BLOCKED: Destructive SQL detected.'; exit 1; fi"
  }]
}
```

**What this does:** Every time Claude tries to run a Bash command, this hook
inspects the command first. If it contains DROP, TRUNCATE, or DELETE FROM,
the hook outputs a BLOCKED message and exits with code 1 — which stops Claude
from running the command at all. Claude never gets to execute the dangerous SQL.

**In plain English:** The hook reads the command Claude is about to run and asks:
"Does this look dangerous?" If yes, it stops Claude before any harm is done.

### PostToolUse Hook
Runs **after** Claude executes a tool action. This type cannot block — the action
has already happened — but it can remind you of important next steps.

**Example in the developer's settings.json:**
```json
{
  "matcher": "Write",
  "hooks": [{
    "type": "command",
    "command": "echo 'File written. Reminder: run tests before raising PR.
                Check eval_checklist.md for generated code quality checks.'"
  }]
}
```

**What this does:** Every time Claude writes a file, this hook prints a reminder
in your terminal. You cannot miss it — it appears immediately after the file
is written, before you move on to the next step.

**In plain English:** The hook taps you on the shoulder every time Claude writes
a file and says "don't forget to run your tests."

### Stop Hook
Runs when a Claude Code session ends (when you type `/exit` or Ctrl+D).
Used to run a final checklist before you close the session.

**Example in the developer's settings.json:**
```json
{
  "hooks": [{
    "type": "command",
    "command": "echo 'Session complete. Before closing:
                (1) run Jest locally,
                (2) check SonarQube gate,
                (3) verify no .env changes staged.'"
  }]
}
```

**What this does:** Every time you end a session, Claude Code prints a three-item
checklist in your terminal as a final reminder before you close.

---

## How to Read a Hook in settings.json

Here is a complete hook configuration from the QA engineer's settings.json,
with annotations:

```json
"PreToolUse": [
  {
    "matcher": "Write",          ← This hook fires when Claude uses the Write tool
    "hooks": [
      {
        "type": "command",
        "command": "if echo \"$CLAUDE_TOOL_INPUT\" | grep -qE 'cy\\.wait\\([0-9]';
                    then echo 'BLOCKED: cy.wait([number]) detected.
                               Use cy.intercept + cy.wait(\"@alias\") instead.';
                    exit 1; fi"
      }
    ]
  }
]
```

**Reading this step by step:**
1. `"matcher": "Write"` — only fires when Claude is writing a file (not on every action)
2. `echo "$CLAUDE_TOOL_INPUT"` — outputs the content Claude is about to write
3. `grep -qE 'cy\.wait\([0-9]'` — searches for the pattern `cy.wait(` followed by a number
4. If found: prints the BLOCKED message and the correct alternative
5. `exit 1` — returns an error code, which tells Claude Code to stop and show the message
6. If not found: the hook exits silently (code 0) and Claude proceeds normally

**What a QA engineer sees when a hook fires:**
```
BLOCKED: cy.wait([number]) detected.
Use cy.intercept + cy.wait("@alias") instead. See SKILL.md for the correct pattern.
```

Claude stops. You see the message. You (or Claude) fix the pattern before the file is written.

---

## Which Personas Have Hooks and Why

Hooks are configured for three personas because they have the highest-stakes
automated risk patterns:

| Persona | Hooks block | Why |
|---|---|---|
| **Developer (Alex)** | DROP/TRUNCATE/DELETE SQL, `.env` file writes | Destructive DB operations and secret leakage are the two most costly mistakes |
| **QA (Jordan)** | `cy.wait([number])`, non-test email addresses, CI pipeline triggers | cy.wait flakiness is a chronic team problem; real customer data in tests is a compliance risk |
| **Ops (Morgan)** | Write/DB commands on production, internal service names in customer-facing docs | Ops has production access; a misfire here has immediate customer impact |

Product Manager, Business Analyst, and Data Analyst do not have blocking hooks
because their outputs are documents that go through human review before being
published — the human review step is the safety mechanism.

---

## What Hooks Cannot Do

Hooks are a safety net, not a substitute for good judgment. They:

- **Cannot catch logical errors** — a hook can block `DROP TABLE` but cannot detect
  a query that silently returns wrong data
- **Cannot verify business correctness** — a hook can warn about internal service
  names but cannot check whether your runbook describes the right fix
- **Cannot replace testing** — a hook can remind you to run tests but cannot
  run them for you or assess whether the test coverage is adequate
- **Cannot prevent all mistakes** — they catch specific, detectable patterns.
  Novel mistakes they have not been configured for will pass through.

Think of hooks as your last automated line of defence — not your only one.

---

## Your Next Step

Go to [07_agents.md](07_agents.md) to learn what agents are, when they are useful,
and how to run them safely.
