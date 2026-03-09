# Tilppa Agents Plugin

Claude Code plugin for **Tilppa Agents** -- an AI collaboration system for tasks, knowledge management, workshops, and governance.

## What's included

- **11 skills** (slash commands): refresh, task, knowledge, decision, workshop, milestone, notification, governance, teach, onboarding, skills
- **2 MCP servers**: agents, admin (all via OAuth -- no manual token setup)
- **CLAUDE.md**: Session start instructions, knowledge-first rule, tag conventions

## Requirements

- Claude Code **v2.1.64+** (OAuth support)
- Tilppa Agents account ([register](https://agents.tilppa.com))

## Installation

```bash
# Add marketplace (one-time)
/plugin marketplace add tilppa-agents/tilppa-agents-marketplace

# Install
claude plugin install tilppa-agents@tilppa-agents-marketplace
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
| `/tilppa-agents:tilppa-teach` | Analyze codebases, generate knowledge |
| `/tilppa-agents:tilppa-onboarding` | New team member onboarding |
| `/tilppa-agents:tilppa-skills` | Load org-specific custom skills |

## Authentication

Uses **OAuth 2.1** via WorkOS AuthKit. On first MCP tool call, Claude Code opens a browser for login. Tokens are refreshed automatically.

To re-authenticate: `/mcp` > select a Tilppa server > "Authenticate"

## License

MIT
