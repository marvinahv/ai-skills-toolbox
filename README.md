# ai-skills-toolbox

A personal small collection of **agent skills** I've written—instruction files that compatible AI coding assistants read to follow repeatable workflows. Hopefully this is useful in your own setup. Skills here are written for tools that load a `SKILL.md` (for example **Cursor** and **Claude Code**).

## What’s included

| Skill | Folder | Summary |
|--------|--------|---------|
| **Jira ticket kickoff** | [`skills/kickoff-jira-ticket/`](skills/kickoff-jira-ticket/) | Fetches a Jira issue (by key or search), ensures a clean git tree, updates `main`, creates a branch `{KEY}-{summary-slug}`, and shows the ticket title and description before you start coding. |

More skills will be added over time; watch the repo or check back in `skills/`.

## Quick start

1. **Pick a skill** — open its folder under [`skills/`](skills/) and read the local `README.md` for requirements, MCP setup, and copy-paste examples.
2. **Install the skill** — copy the skill directory to the location your IDE expects (user-wide or project-only paths are documented in each skill’s README).
3. **Configure integrations** — for example, the Jira kickoff skill expects an **Atlassian MCP** server so the assistant can read issues.

Detailed steps, example prompts, and branch-naming rules live in each skill’s documentation, not duplicated here.

## Requirements (general)

- An AI IDE or CLI that **loads agent skills** from `SKILL.md`
- Any **MCP servers** or credentials described in the skill you install

## Contributing

Suggestions and pull requests are welcome. If you plan a larger change, opening an issue first helps align on scope.

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).

---
