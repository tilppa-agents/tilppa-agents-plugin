# CLAUDE.md -- Tilppa Agents Plugin

Tilppa Agents is an AI collaboration system for tasks, knowledge management, workshops, and governance.

## Session Start

**At the START of every conversation**, run `/tilppa-agents:tilppa-refresh`.

This loads: user info, notifications, active roles, open tasks, knowledge index, and recent decisions.

**IMPORTANT: Output the full refresh result as text** -- tool output collapses in the Claude Code UI.

## Available Skills

| Skill | Command | When to use |
|-------|---------|-------------|
| refresh | `/tilppa-agents:tilppa-refresh` | Session start, context reload |
| task | `/tilppa-agents:tilppa-task` | Create, update, search, list tasks |
| knowledge | `/tilppa-agents:tilppa-knowledge` | Search/save knowledge |
| decision | `/tilppa-agents:tilppa-decision` | Record/search decisions and rationale |
| workshop | `/tilppa-agents:tilppa-workshop` | Multi-agent workshops for planning |
| milestone | `/tilppa-agents:tilppa-milestone` | Track progress, manage templates |
| notification | `/tilppa-agents:tilppa-notification` | Reminders, messages, pending checks |
| governance | `/tilppa-agents:tilppa-governance` | Clearance, shaping, trust, policies |
| skills | `/tilppa-agents:tilppa-skills` | Load org-specific custom skills |
| org | `/tilppa-agents:tilppa-org` | Multi-org management, context switching |
| teach | `/tilppa-agents:tilppa-teach` | Analyze codebases, generate knowledge |
| onboarding | `/tilppa-agents:tilppa-onboarding` | New team member onboarding |

## Knowledge-First Rule

**ALWAYS check the knowledge database BEFORE reading source code.**

1. `/tilppa-agents:tilppa-knowledge` -- search for relevant knowledge
2. `/tilppa-agents:tilppa-decision` -- search for related decisions
3. Only then read source code if knowledge does not cover the topic

- **Knowledge** = current state (how things work now)
- **Decisions** = historical record (what was decided and why)

## Rules

**NEVER:**
- Skip the knowledge-first check before reading code
- Assume infrastructure details -- verify with knowledge DB first
- Shorten UUIDs -- always use full IDs for knowledge, decisions, tasks, and sessions

**ALWAYS:**
- Use subagents to parallelize independent work
- Adapt communication style to the user (check `whoami` and contacts)

## Git Commits

All commit messages MUST be in English.

## Tag Convention

All tags follow `prefix:value` format: `scope:`, `project:`, `feature:`, `epic:`, `version:`, `style:`, `skills:`, `category:`, `jira:`, `pr:`, `sprint:`, `system:`

## MCP Servers

4 MCP servers connect automatically via OAuth (no manual token configuration needed):
- `tilppa-agents` -- core: tasks, knowledge, decisions, workshops, governance
- `tilppa-contacts` -- contact management
- `tilppa-admin` -- user management, audit, settings, clearance
- `tilppa-teach` -- codebase analysis and knowledge generation

## Authentication

This plugin uses **OAuth 2.1** via WorkOS AuthKit. On first use, Claude Code opens a browser for login. Tokens are managed automatically.

To re-authenticate: `/mcp` > select server > "Authenticate"

## Troubleshooting

If MCP tools are missing or `/tilppa-agents:tilppa-refresh` fails:

1. **Plugin enabled?** -- `/plugin` > check tilppa-agents is enabled
2. **Authenticated?** -- `/mcp` > check server status, re-authenticate if needed
3. **Claude Code version?** -- Requires v2.1.64+ for OAuth support
4. **Server running?** -- `curl https://agents.tilppa.com/health`
5. **Not registered?** -- Register at https://agents.tilppa.com
