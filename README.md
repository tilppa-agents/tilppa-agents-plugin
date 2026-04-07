# Tilppa Agents Plugin (Cowork)

Claude Cowork plugin for **Tilppa Agents** — an AI collaboration system for tasks, knowledge management, workshops, and governance.

> **Branch:** `feature/cowork-support` — Cowork-optimized version. For Claude Code version, see `main` branch.

## What's included

- **12 skills** (slash commands): refresh, task, knowledge, decision, workshop, milestone, notification, governance, teach, onboarding, org, skills
- **2 MCP servers**: agents, admin (all via OAuth — no manual token setup)
- **CLAUDE.md**: Cowork-specific session instructions, knowledge-first rule, Cowork interaction patterns

## Cowork vs Claude Code

| Aspect | This branch (Cowork) | main branch (Code) |
|--------|---------------------|-------------------|
| Environment | Claude Cowork desktop app | Terminal / VS Code |
| User interaction | `AskUserQuestion` for clarification | Text prompts |
| Progress tracking | `TodoWrite` for task lists | Subagents for parallelism |
| File delivery | `present_files` for documents | Terminal output |
| Document creation | Cowork skills (docx, pptx, xlsx, pdf) | Direct file writes |
| MCP config | `.mcp.json` active (OAuth) | `.mcp.json.disabled` |

## Requirements

- Claude Cowork desktop app
- Tilppa Agents account ([register](https://agents.tilppa.com))

## Installation

Install as a Cowork plugin:

1. Download the `.plugin` file from releases
2. Open Claude Cowork → Settings → Plugins → Install
3. Run `/tilppa-agents:tilppa-refresh` to start

## First use

1. Install the plugin
2. Start a Cowork session
3. Run `/tilppa-agents:tilppa-refresh`
4. A browser opens for OAuth login (first time only)
5. You're connected — start using skills

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
| `/tilppa-agents:tilppa-org` | Multi-org management and switching |

## Multi-org setup

This plugin works across all your Tilppa organizations. Use `/tilppa-agents:tilppa-org` to switch between orgs. Each org has its own tasks, knowledge, decisions, and governance rules.

## Authentication

Uses **OAuth 2.1** via WorkOS AuthKit. On first MCP tool call, a browser opens for login. Tokens are refreshed automatically.

## License

MIT
