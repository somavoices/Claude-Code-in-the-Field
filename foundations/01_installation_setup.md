# 01 — Installation & Setup
## Getting Claude Code Running on Your Machine

---

## What You Will Accomplish

By the end of this document you will have:
- Claude Code installed and running
- Your account authenticated
- Claude responding to your first message
- Your persona's context files ready to load

**Estimated time: 20–30 minutes.**

---

## Prerequisites

Before installing Claude Code you need:

| Requirement | How to check |
|---|---|
| A computer running Windows, macOS, or Linux | — |
| Node.js version 18 or higher | Run `node --version` in your terminal |
| npm (comes with Node.js) | Run `npm --version` in your terminal |
| An Anthropic account | Sign up at console.anthropic.com |
| An Anthropic API key | Created in your Anthropic console account |

**If you do not have Node.js:** Download it from nodejs.org. Install the LTS
(Long Term Support) version. This also installs npm automatically.

**If you do not have an Anthropic account:** Your organisation may provide a
shared API key. Check with your training facilitator before creating a personal account.

---

## Step 1 — Install Claude Code

Open your terminal (Command Prompt or PowerShell on Windows, Terminal on macOS/Linux)
and run:

```bash
npm install -g @anthropic-ai/claude-code
```

The `-g` flag installs Claude Code globally so you can run it from any folder.

**Verify the installation worked:**
```bash
claude --version
```

You should see a version number printed. If you see "command not found", the
installation did not complete — try running the install command again.

---

## Step 2 — Authenticate

Claude Code needs your Anthropic API key to communicate with Claude.

**Run the authentication command:**
```bash
claude
```

The first time you run this, Claude Code will prompt you to authenticate.
Follow the on-screen instructions — it will open a browser window to log in
to your Anthropic account, or ask you to paste an API key directly.

**If your organisation uses a shared API key:**
```bash
export ANTHROPIC_API_KEY="your-api-key-here"
```

On Windows (PowerShell):
```powershell
$env:ANTHROPIC_API_KEY = "your-api-key-here"
```

Ask your facilitator whether to use a personal or shared API key.

---

## Step 3 — Verify Claude Is Responding

Once authenticated, you should see the Claude Code prompt in your terminal.
Type a simple message to confirm everything is working:

```
> Hello, can you confirm you are working?
```

Claude should respond with a short confirmation message. If it does, you are ready.

**If Claude does not respond:**
- Check your internet connection
- Verify your API key is correct in the Anthropic console
- Check that your organisation's network does not block API calls

---

## Step 4 — Navigate to Your Working Folder

Claude Code works best when you run it from the folder that contains your work.
For this training, navigate to the training kit folder:

```bash
cd path/to/claude_code_persona
```

Running Claude Code from a folder means it can read files in that folder
when you ask it to. This becomes important in Phase 3 when you connect tools.

---

## Step 5 — Understand the Claude Code Interface

When Claude Code is running you will see a prompt like this:

```
>
```

This is where you type your messages. A few things to know:

- **Press Enter** to send your message
- **Press Ctrl+C** to cancel the current response
- **Type `/help`** to see available commands
- **Type `/exit`** or press Ctrl+D to quit
- **Multi-line input:** Press Shift+Enter for a new line without sending

---

## Step 6 — Load Your Persona Context Files

This is how you give Claude the background it needs to behave as your role-specific
assistant. You do this at the start of each session.

Open a new Claude Code session from the `claude_code_persona` folder and run:

```
> /init
```

This reads the CLAUDE.md file in your current folder and loads it as context.
Claude will acknowledge the context and be ready to work.

**Loading your persona-specific files:**

For your role folder (e.g., `personas/developer/`), you can reference the files
directly in your first message:

```
> Please read and load the following files as your context for this session:
  - personas/developer/CLAUDE.md
  - personas/developer/memory.md
  - personas/developer/SKILL.md

  Confirm when you have read all three and summarise who you are and what
  we are working on.
```

Claude will read all three files and respond with a summary. From that point
on in the session, it behaves as your role-specific assistant.

---

## Step 7 — Setup Checklist

Before moving on to the next foundation document, confirm each item:

- [ ] `claude --version` returns a version number
- [ ] Claude responded to your "Hello" test message
- [ ] You can navigate to your working folder in the terminal
- [ ] You successfully loaded one persona's CLAUDE.md and Claude summarised the context
- [ ] You know how to exit Claude Code (Ctrl+D or `/exit`)

**All checked?** You are ready for [02_core_concepts.md](02_core_concepts.md).

---

## Common Setup Problems

| Problem | Likely cause | Fix |
|---|---|---|
| `command not found: claude` | Installation did not complete | Re-run `npm install -g @anthropic-ai/claude-code` |
| `authentication failed` | API key is wrong or expired | Check key in console.anthropic.com |
| `rate limit exceeded` | Too many requests from shared key | Ask facilitator for a new key or wait |
| Claude responds very slowly | Network latency or high API load | Normal — responses can take 5–30 seconds |
| `/init` says no CLAUDE.md found | You are not in the right folder | Run `cd path/to/claude_code_persona` first |
