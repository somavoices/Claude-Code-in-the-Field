# 02 — Core Concepts
## Prompts, Context, Tokens, Memory, and Skills

> **Status: Draft — content under review**

---

## What You Will Learn

This document explains the five foundational concepts that everything else in this
training kit builds on. Read it before using any persona files.

---

## Concept 1 — What Is a Prompt?

A prompt is the message you send to Claude. It is not just a question — it is
the complete instruction that tells Claude what to do, who you are, what rules
to follow, and what input to work with.

**A weak prompt:**
```
Review this code.
```

Claude has no idea what language, what standards to apply, what to look for,
or who will use the output. It will produce a generic response.

**A strong prompt:**
```
Review this TypeScript function for:
- Missing error handling on async operations
- N+1 database query patterns
- TypeScript strict mode violations (no 'any', no implicit returns)

The function is part of a Node.js dispatch service. Any issue you flag should
include the line, the risk, and a specific fix.

[PASTE CODE HERE]
```

Claude now knows the language, the standards, the context, and exactly what to
produce. The output will be specific, actionable, and immediately useful.

**The rule:** The more specific and complete your prompt, the better the output.
Vague prompts produce vague responses. Specific prompts produce specific responses.

---

## Concept 2 — What Is Context?

Context is the background information Claude has available when it responds.
It includes everything you have said in the conversation, every file you have
asked it to read, and every instruction you have given it.

**Why context matters so much:**

Claude does not know anything about your specific situation by default. It knows
general facts about software, data, business processes, and writing — but it
does not know:
- What company you work for or what product you build
- Your team's coding conventions
- The projects currently in progress
- Who your colleagues are and what their preferences are
- The incident that happened last month

Without context, Claude behaves like a smart stranger. With context, it behaves
like a knowledgeable colleague.

**How context gets into Claude:**

1. **What you type in the conversation** — anything you write is context
2. **Files you ask Claude to read** — CLAUDE.md is auto-loaded; memory.md you load on demand
3. **Files Claude reads via tools** — when MCP is configured, Claude can read
   your actual code files, query your database, search documentation

**A practical example:**

*Without context:*
```
You: Write a dbt model for job completions.
Claude: Here is a generic dbt model for tracking job completions...
        [produces a model with made-up column names, generic structure]
```

*With context (memory.md loaded):*
```
You: Write a dbt model for job completions.
Claude: Based on the dispatch_jobs source table, here is a staging model
        that filters cancelled jobs, casts scheduled_at and completed_at
        to timestamp_ntz, and deduplicates on job_id...
        [produces a model that matches your actual schema and conventions]
```

Same prompt. Completely different output. The difference is context.

---

## Concept 3 — What Are Tokens and Why Do They Matter?

A token is a unit of text — roughly 4 characters or 0.75 words. Tokens matter
for two reasons:

**1. Context window limit:**
Claude can only hold a certain number of tokens in memory at once — this is
called the context window. Think of it like a whiteboard: you can write a lot
on it, but eventually it fills up. When the context window is full, Claude
cannot hold earlier parts of the conversation.

In practice this means:
- Very long conversations can cause Claude to "forget" early instructions
- Very long files loaded as context consume space that could be used for responses
- If you are working on a large codebase, load only the relevant files

**2. Cost:**
API usage is billed by tokens. More tokens = higher cost. For most daily tasks
this is negligible, but running large automated agent sessions on big codebases
can generate significant token usage. Your organisation may set limits.

**Practical implications for you:**
- Keep CLAUDE.md and memory.md focused — not exhaustive diaries
- If Claude seems to "forget" earlier instructions mid-conversation, start a
  fresh session and re-load your context files
- When asking Claude to review code, paste the relevant function — not the
  entire file — unless the full file is necessary

---

## Concept 4 — How Does Claude "Remember" Things?

This is one of the most misunderstood aspects of working with Claude.

**Claude has no persistent memory between sessions.**

Every time you start a new Claude Code session, Claude starts completely fresh.
It does not remember the conversation you had yesterday, the code it reviewed
last week, or the bug it helped you fix last month.

**Within a single session, Claude remembers everything you have said.**
It keeps the full conversation history in the context window. If you tell Claude
something at the start of a session, it will remember it until the session ends.

**So how do you give Claude memory?**

You create memory explicitly — in files. That is exactly what `memory.md` is.
It is a written record of the things Claude should know, stored in a file that
you load at the start of each session.

Think of it this way:
- **Session memory** = Claude remembers what you said *in this conversation*
- **Persistent memory** = You write it in memory.md and load it next session

**What belongs in memory.md:**
- Current project status and key decisions
- Domain rules specific to your company
- People — who to loop in for what, how they prefer to work
- Past incidents and what was learned
- Things you would otherwise have to re-explain every session

**What does NOT belong in memory.md:**
- General technical knowledge (Claude already knows this)
- Things that change every day (keep memory.md stable)
- Everything — memory.md should be focused, not a complete diary

---

## Concept 5 — What Are Skills, Context Files, and How Do They Relate?

The training kit uses three types of context files. Understanding the difference
helps you know which to load when.

### CLAUDE.md — Your Identity and Rules
Tells Claude who you are, what you are working on right now, your conventions,
and your absolute constraints. Load this at the start of every session.

Think of it as: **your briefing document** — what a new colleague would read
on their first day to understand your role.

### memory.md — Project Knowledge and History
Gives Claude deep knowledge about your specific projects, domain, colleagues,
and past incidents. Load this alongside CLAUDE.md.

Think of it as: **the notes you would give a colleague who is taking over your work** —
current status, decisions made, people involved, things to watch out for.

### Skills — How to Do One Task Really Well
Skills teach Claude the exact steps, format, and conventions for one high-value task
(writing a PR description, writing a dbt model, writing a runbook, etc.).
They live in `.claude/skills/` and are available as slash commands — type `/write-pr-description`
and Claude runs that skill automatically. No manual loading required.

Think of it as: **a procedure manual pre-loaded as a command** — step-by-step instructions
so the output matches your team's standards without you having to explain them every time.

---

## How These Concepts Connect

Here is how a well-structured Claude Code session uses all five concepts:

```
START OF SESSION
│
├─ CLAUDE.md auto-loads    → Context: who you are, your rules
├─ Load memory.md          → Context: project knowledge, people, incidents (on demand)
├─ Type /skill-name        → Skill auto-applies task conventions
│
└─ Write a prompt          → Specific task + relevant input
   │
   ├─ Claude uses all context to produce output
   │
   ├─ You evaluate with safety_checklist.md
   │
   ├─ If output is good: use it
   │
   └─ If output needs improvement:
      ├─ Identify what is missing or wrong
      ├─ Add that specific thing to your prompt
      └─ Re-run → better output
```

The session ends. Claude forgets everything.
Next session: reload context files and start again.

---

## The Most Important Insight

Most people who get poor results from Claude are not giving it enough context.
They type a short question, get a generic answer, and conclude that Claude
is not very useful.

The people who get excellent results give Claude:
1. Role context (who they are and what they need)
2. Project context (what they are working on specifically)
3. Convention context (how their team does things)
4. A specific, well-structured prompt
5. The actual input to work with

That is what this entire training kit is built to provide. The CLAUDE.md, memory.md,
skills, and prompt_library.md files do the hard work of assembling the right
context — so you do not have to type it from scratch every time.

---

## Your Next Step

Go to [03_tools_and_mcp.md](03_tools_and_mcp.md) to learn how Claude Code
connects to your actual tools — GitHub, your filesystem, your database.
