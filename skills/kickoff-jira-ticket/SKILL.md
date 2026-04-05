---
name: kickoff-jira-ticket
description: Runs the Git and JIRA setup when starting work on a new JIRA issue. Use when the user wants to work on a JIRA ticket, start a new ticket, or says something like "let's work on KAN-6" or "start JIRA ticket".
---

# Git Workflow for New JIRA Issues

When starting work on a new JIRA issue, follow these steps in order. The JIRA ticket must be in context before any Git steps, so we can use its key and summary for the branch name and display details at the end.

## 1. Get the JIRA ticket in context

**Before any Git operations**, obtain the JIRA issue so we have its key, summary, and description for later steps.

Use the **Atlassian MCP server** for all JIRA operations. Choose the most appropriate tool(s) from that server for the situation:

- **If the user gave a ticket key** (e.g. "KAN-6", "let's work on KAN-6"): use the Atlassian MCP server to fetch that issue by key so we have key, summary, and description.
- **If the user did not give a key** (e.g. "the revenue screen ticket"): use the Atlassian MCP server to search for the issue (e.g. by summary text or JQL), identify the correct issue key, then fetch that issue so we have key, summary, and description.

From the fetched issue, keep in context:

- **Key** (e.g. `KAN-6`) — for the branch name
- **Summary** — for the branch slug and for the final display
- **Description** — for the final display

Do not continue to Git steps until the JIRA ticket is loaded and you have the key and summary. If the ticket cannot be found or the user has not specified which ticket to use, stop and ask for clarification.

## 2. Verify no pending changes

```bash
git status
```

**If there are uncommitted changes:** stop the process. Tell the user they need to take action before proceeding, and show the available options:

- **Commit** — `git add` then `git commit` to keep changes on the current branch
- **Stash** — `git stash` (optionally `git stash push -m "message"`) to save changes for later
- **Discard** — `git checkout -- .` (tracked files) or remove untracked files; changes are lost

Do not continue to the next steps until the working tree is clean (or the user has resolved and confirmed).

Once the user has confirmed that is ready to continue, verify step no. 2 again until we confirm is resolved.

## 3. Checkout main

```bash
git checkout main
```

## 4. Pull latest changes

```bash
git pull
```

## 5. Create and checkout the feature branch

Using the JIRA ticket already in context (from step 1):

- **Branch name convention**: `{KEY}-{slug-of-summary}`
- **Slug**: lowercase, words separated by hyphens (e.g. "User can see revenue screen" → `user-can-see-revenue-screen`)

**Example**: For JIRA ticket `KAN-6` with summary "User can see revenue screen":

```bash
git checkout -b KAN-6-user-can-see-revenue-screen
```

## 6. Verify branch

```bash
git status
```

Confirm you are on the new branch with a clean working tree before starting work.

## 7. Display JIRA ticket summary to the user

Using the JIRA ticket already fetched in step 1, display to the user:

- **Title (summary)** of the ticket
- **Description** of the ticket

Present this in a clear, readable format so the user can confirm context before starting work. No need to fetch the issue again — use the data from step 1.

---

**Summary order**: (1) Get JIRA ticket in context (search/fetch so we have key + summary + description) → (2) `git status` — if dirty, stop and list actions (commit / stash / discard) → (3) `git checkout main` → (4) `git pull` → (5) `git checkout -b {KEY}-{slug-of-summary}` → (6) `git status` → (7) Show summary + description from the ticket already in context
