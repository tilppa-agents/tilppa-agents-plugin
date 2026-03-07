# Tilppa Agents Plugin

Claude Code plugin for **Tilppa Agents** -- an AI collaboration system for tasks, knowledge management, workshops, and governance.

## What's included

- **12 skills** (slash commands): refresh, task, knowledge, decision, workshop, milestone, notification, governance, org, teach, onboarding, skills
- **4 MCP servers**: agents, contacts, admin, teach (all via OAuth -- no manual token setup)
- **CLAUDE.md**: Session start instructions, knowledge-first rule, tag conventions

## Requirements

- Claude Code **v2.1.64+** (OAuth support)
- Tilppa Agents account ([register](https://agents.tilppa.com))

## Installation

```bash
claude plugin install tilppa-agents
```

Or install from local directory for development:

```bash
claude --plugin-dir /path/to/tilppa-agents-plugin
```

## First use

1. Install the plugin
2. Start Claude Code in your project
3. Run `/tilppa-agents:tilppa-refresh`
4. Claude Code opens a browser for OAuth login (first time only)
5. You're connected -- start using skills

## Skills

| Command | Description |
|---------|-------------|
| `/tilppa-agents:tilppa-refresh` | Load session context (run at conversation start) |
| `/tilppa-agents:tilppa-task` | Create, update, search, list tasks |
| `/tilppa-agents:tilppa-knowledge` | Search and save knowledge |
| `/tilppa-agents:tilppa-decision` | Record and search decisions |
| `/tilppa-agents:tilppa-workshop` | Multi-agent workshops for planning |
| `/tilppa-agents:tilppa-milestone` | Track progress and milestones |
| `/tilppa-agents:tilppa-notification` | Reminders and messages |
| `/tilppa-agents:tilppa-governance` | Clearance, trust, policies |
| `/tilppa-agents:tilppa-org` | Multi-org management |
| `/tilppa-agents:tilppa-teach` | Analyze codebases, generate knowledge |
| `/tilppa-agents:tilppa-onboarding` | New team member onboarding |
| `/tilppa-agents:tilppa-skills` | Load org-specific custom skills |

## Authentication

Uses **OAuth 2.1** via WorkOS AuthKit. On first MCP tool call, Claude Code opens a browser for login. Tokens are refreshed automatically.

To re-authenticate: `/mcp` > select a Tilppa server > "Authenticate"

## Plugin vs Base Repo

This plugin is a lightweight alternative to the [tilppa-agents-base](https://github.com/pekkaliu/tilppa-agents-base) git repo.

| Feature | Plugin | Base Repo |
|---------|--------|-----------|
| Installation | `/plugin install` | `git clone` |
| Authentication | OAuth (automatic) | Bearer token (manual) |
| Skills | Included | Included |
| Org workspace (`_workspace/`) | Not included | Included |
| Org rules (`.claude/rules/`) | Not included | Included |
| Subproject support (`setup.sh`) | Not needed | Included |

Use the **plugin** for individual use. Use the **base repo** for team/org-level workflows.

## License

MIT
