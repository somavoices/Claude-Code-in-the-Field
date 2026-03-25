# 03 — Tools and MCP
## Connecting Claude to Your Real Work Environment

> **Status: Draft — content under review**

---

## What You Will Learn

How Claude Code connects to external tools — your filesystem, GitHub, databases,
and documentation. What MCP is, why it exists, and how to think about permissions.

---

## The Problem MCP Solves

Without tool connections, Claude only knows what you paste into the conversation.
If you want Claude to review a function in your codebase, you have to copy it
manually. If you want Claude to understand your database schema, you have to
paste the schema. If you want Claude to check documentation, you have to find
and paste the relevant page.

This is workable for small tasks but becomes tedious and error-prone for larger ones.
What if you could give Claude direct access to the things it needs?

That is what MCP does.

---

## What Is MCP?

MCP stands for **Model Context Protocol**. It is a standard way for Claude Code
to connect to external tools and services.

Think of MCP as a set of authorised channels between Claude and your work environment:

```
Without MCP:                    With MCP:

You paste code → Claude         Claude reads file directly
You paste schema → Claude       Claude queries DB schema
You paste docs → Claude         Claude searches documentation
You paste PR → Claude           Claude reads GitHub PR
```

Each MCP connection is called an **MCP server**. Each server handles one type
of tool (filesystem, GitHub, database, web search, etc.).

---

## How MCP Works — Step by Step

1. **You configure an MCP server** in your `.mcp.json` file, specifying
   which tool to connect and what credentials to use

2. **Claude Code starts the server** when you begin a session

3. **When you ask Claude to do something** that requires the tool
   (e.g., "read the file src/dispatch/job-assignment.ts"), Claude asks the
   MCP server for that resource

4. **The MCP server retrieves the resource** and returns it to Claude

5. **Claude uses the resource** as context — exactly as if you had pasted it in

The key point: **you stay in control**. You decide which tools to connect,
which operations are allowed, and which are denied. Claude cannot access anything
you have not explicitly configured.

---

## The Four Most Common MCP Servers in This Kit

### 1. Filesystem Server
Gives Claude the ability to read and write files in your local working directory.

**What it enables:**
- Reading your code files to understand existing patterns before generating new code
- Reading existing test specs before writing new tests
- Writing generated files directly to the correct location

**The safety boundary:**
- You specify exactly which folders Claude can access: `src/`, `tests/`, `runbooks/`
- You explicitly deny sensitive paths: `.env`, `**/*secret*`, `infrastructure/`

**Example — without filesystem MCP:**
```
You: Write a Cypress test for the job assignment feature.
     [You have to manually paste in existing test examples for style reference]
```

**Example — with filesystem MCP:**
```
You: Read the existing Cypress tests in apps/web/cypress/e2e/dispatch/ and then
     write a new test for job assignment following the same patterns.
Claude: [Reads 3 existing files] I can see you use data-testid selectors,
        cy.intercept for network calls, and cy.task for test data setup.
        Here is the new test following those patterns...
```

### 2. GitHub Server
Gives Claude read access to your GitHub repositories and pull requests.

**What it enables:**
- Reading code across repos without copy-pasting
- Reviewing PR diffs with full context
- Searching for existing implementations before writing new code

**The safety boundary:**
- Read operations (read file, get PR, search code) are allowed
- Write operations (merge PR, push to main, delete branch) are denied
- Claude can never merge your code or push without your explicit action

### 3. Database Server (PostgreSQL)
Gives Claude read-only access to query your staging database schema.

**What it enables:**
- Understanding your actual table structure before writing SQL
- Running EXPLAIN ANALYZE on query plans before recommending them
- Validating that a migration is consistent with the current schema

**The safety boundary:**
- SELECT queries only — INSERT, UPDATE, DELETE, DROP are all denied
- Staging database only — never production database URL
- Claude reads schema and query plans; it never modifies data

### 4. Web Search Server (Brave Search)
Gives Claude the ability to search current documentation and technical references.

**What it enables:**
- Looking up the correct API for a library version you are using
- Finding official documentation for tools like dbt, Cypress, Datadog
- Verifying that a recommended approach is current (not deprecated)

**Why this matters:** Claude's training data has a cutoff date. Library APIs change.
Framework versions differ. Web search lets Claude verify against current docs
rather than relying on potentially outdated training knowledge.

---

## Allowed Operations vs. Denied Operations

Every MCP server in this kit specifies what Claude is and is not allowed to do.
This is not optional — it is the security model.

**Why explicitly deny things?**

Because Claude's job is to be helpful, and "being helpful" with a database
connection might mean writing a query that accidentally deletes data. Explicit
denials prevent Claude from even attempting operations that could cause harm.

**Real example from the developer's `.mcp.json`:**
```json
"postgres": {
  "command": "npx",
  "args": ["-y", "@modelcontextprotocol/server-postgres"],
  "env": {
    "POSTGRES_CONNECTION_STRING": "${STAGING_DB_URL}"
  }
}
```

The MCP server itself enforces read-only access — `STAGING_DB_URL` points to the staging database, never production. What Claude can and cannot do with that connection is enforced through `settings.json` permissions and hooks, not inside the MCP config.

Claude can read, inspect, and plan. It cannot change anything.

**The principle:** Give Claude the minimum access needed to do the task.
Read-only is always safer than read-write. Scoped folder access is safer
than full filesystem access. Explicit allow-lists are safer than implicit trust.

---

## How to Configure MCP for Your Role

Each persona folder contains a `.mcp.json` file with pre-configured MCP servers
for that role. Claude Code loads this file automatically when launched from that folder.

**Step 1 — Review the config file**
Open your persona's `.mcp.json` and read it. Each entry under `mcpServers` is one
tool connection — it specifies the command to run, its arguments, and any environment
variables needed.

**Step 2 — Set environment variables**
The config references environment variables like `${GITHUB_TOKEN}` and
`${BRAVE_API_KEY}`. These keep credentials out of the config file.

```bash
export GITHUB_TOKEN="your-github-personal-access-token"
export BRAVE_API_KEY="your-brave-api-key"
```

Ask your facilitator which API keys your organisation provides.

**Step 3 — Launch Claude Code from your persona folder**
```bash
cd personas/developer    # or your role folder
claude
```

Claude Code reads `.mcp.json` automatically at startup and connects the servers.
No manual loading or flag needed.

**Step 4 — Test the connection**
Ask Claude to use the tool in a simple, safe way:

```
> List the files in the src/ folder.
```

If MCP is working, Claude will list the actual files. If not, it will say
it cannot access the filesystem.

---

## A Note on Security

MCP servers run as local processes on your machine. They do not send your code
or data to third parties — they only pass information between your local
environment and Claude.

However, you should still follow these practices:
- Never put API keys or passwords directly in `.mcp.json` — use environment variables
- Never configure MCP with production database credentials — staging only
- Review allowed operations in your config before enabling a new MCP server
- If you are unsure whether an MCP operation is safe, check with your team lead

---

## Your Next Step

Go to [04_your_first_prompt.md](04_your_first_prompt.md) for a hands-on walkthrough
of writing, running, evaluating, and improving your first prompt.
