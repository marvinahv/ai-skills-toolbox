# kickoff-jira-ticket

An **AI skill** that automates the git setup when you start work on a JIRA ticket. It is designed to work in **any compatible AI IDE** that loads agent skills from a `SKILL.md` (for example **Cursor** and **Claude Code**). Instead of manually looking up the ticket, switching branches, and pulling the latest code, you tell the assistant to start a ticket (e.g. “let’s work on KAN-6”) and it handles the workflow for you.

## What it does

When you invoke this skill, the assistant will:

1. **Fetch the JIRA ticket** — looks up the issue by key (or searches for it by description if you don’t know the key), and loads the title and description into context
2. **Check for uncommitted changes** — if your working tree is dirty, it stops and shows you your options (commit, stash, or discard) before continuing
3. **Checkout and update `main`** — switches to `main` and pulls the latest changes
4. **Create a feature branch** — names it using the JIRA key and a slug of the ticket summary (e.g. `KAN-6-user-can-see-revenue-screen`)
5. **Confirm the branch** — runs `git status` so you can see you’re on the right branch with a clean tree
6. **Display the ticket** — shows the ticket title and description so you can review the work before you start

## Examples (Claude Code and Cursor)

The phrasing below works the same idea in either IDE: the AI reads `SKILL.md` and follows it using your configured Atlassian MCP tools.

### Claude Code

```
You: let's work on KAN-42
```

The assistant fetches the JIRA ticket, ensures your repo is clean, creates branch `KAN-42-add-payment-confirmation-screen`, and shows you the ticket details — ready to code.

```
You: start the revenue screen ticket
```

The assistant searches JIRA, identifies the right issue, and runs through the same setup.

### Cursor

```
You: let's work on KAN-42
```

Same behavior: fetch ticket, clean tree, create `KAN-42-…` branch from updated `main`, then show title and description.

```
You: kickoff JIRA ticket for the login bug
```

The assistant uses JIRA search if needed, then performs the same git steps.

In both IDEs, natural variants like `start JIRA ticket KAN-6` or `kickoff the revenue screen ticket` are fine as long as the skill is installed and the Atlassian MCP server is available to the assistant.

## Requirements

- A **compatible AI IDE** with agent skill support (this repo is tested with **Claude Code** and **Cursor**)
- The **Atlassian Rovo MCP server** (or equivalent Atlassian MCP) configured **in that IDE** so the assistant can call JIRA
- An Atlassian Cloud site with Jira
- A git repository with a `main` branch

## Setup

### 1. Install your AI IDE

- **Claude Code:** [Claude Code installation guide](https://docs.anthropic.com/en/docs/claude-code/getting-started)
- **Cursor:** [Cursor](https://cursor.com) — install the editor; skills are loaded from the paths below when present

### 2. Configure the Atlassian Rovo MCP server

This skill uses the [Atlassian Remote MCP Server](https://support.atlassian.com/atlassian-rovo-mcp-server/docs/getting-started-with-the-atlassian-remote-mcp-server/) to read JIRA issues. Add the MCP server using **your IDE’s MCP configuration** (Claude Code config file or Cursor MCP settings).

Example server definition (adapt the enclosing file to Claude Code vs Cursor as documented by each product):

```json
{
  "mcpServers": {
    "Atlassian": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote@latest",
        "https://mcp.atlassian.com/v1/mcp"
      ]
    }
  }
}
```

Authentication is handled via OAuth 2.1 — on first use, your browser will open and prompt you to authorize access to your Atlassian account. No API tokens or manual credentials are required.

> **Note:** Node.js v18 or later is required.

### 3. Install this skill

Place the skill so your IDE discovers `SKILL.md` (names and locations follow each product’s agent-skill conventions):

**Claude Code**

- User-wide: `~/.claude/skills/kickoff-jira-ticket/SKILL.md`
- Project-only: `.claude/skills/kickoff-jira-ticket/SKILL.md` in the repo

**Cursor**

- User-wide: `~/.cursor/skills/kickoff-jira-ticket/SKILL.md`
- Project-only: `.cursor/skills/kickoff-jira-ticket/SKILL.md` in the repo

Copy the `kickoff-jira-ticket` folder so `SKILL.md` (and optional `README.md`) live under one of those paths. Do not use `~/.cursor/skills-cursor/` — that directory is reserved for Cursor’s built-in skills.

After installation, **invoke the skill in chat** using the examples above; the assistant should follow `SKILL.md` when the task matches starting or continuing work on a JIRA ticket.

## Usage

Trigger the skill naturally in **Claude Code** or **Cursor**:

| What you say | What happens |
|---|---|
| `let's work on KAN-6` | Fetches KAN-6 and sets up the branch |
| `start JIRA ticket KAN-42` | Same as above |
| `kickoff the login bug ticket` | Searches JIRA by description and sets up the branch |

## Branch naming

Branches follow the pattern `{KEY}-{slug-of-summary}`:

| JIRA Key | Summary | Branch |
|---|---|---|
| KAN-6 | User can see revenue screen | `KAN-6-user-can-see-revenue-screen` |
| PROJ-101 | Fix null pointer in Auth service | `PROJ-101-fix-null-pointer-in-auth-service` |

## Contributing

Contributions are welcome. Please open an issue to discuss what you'd like to change before submitting a pull request.

## License

MIT License

Copyright (c) 2026 Marvin Alejandro Herrera Vizuett

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
