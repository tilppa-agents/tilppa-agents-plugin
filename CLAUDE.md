# CLAUDE.md -- Tilppa Agents Plugin (Cowork)

Tilppa Agents is an AI collaboration system for tasks, knowledge management, workshops, and governance. This plugin is optimized for **Claude Cowork** (desktop app).

## Session Start

**Do NOT auto-run `/tilppa-agents:tilppa-refresh`.** Before your first response, call:

1. `mcp__tilppa-agents__whoami` — load user profile (required by NEVER rule below)
2. `mcp__tilppa-agents__notifications_query` mode: "pending" — surface pending alerts

This pair (~200 tokens) replaces the full refresh (~5–10 kT per call). Show pending notifications first if any exist.

Run `/tilppa-agents:tilppa-refresh` **only on explicit request** — e.g. status overview, or an ad-hoc session without a clear task. `workshop_start` already returns relevant tasks/knowledge/decisions semantically, so a general refresh is rarely needed when you begin with a TP-task.

## Cowork Interaction Patterns

**ALWAYS use these Cowork-native tools:**

- `AskUserQuestion` — Clarify requirements before starting multi-step work
- `TodoWrite` — Track progress for any task with 3+ steps
- `present_files` — Share deliverables (documents, reports, exports) with the user
- Cowork file skills (docx, pptx, xlsx, pdf) — When producing documents, use the built-in Cowork skills for professional output

**Output behavior:**
- Refresh results display naturally in Cowork — no need to force-print as text
- Save all outputs to the mounted workspace folder so the user can access them
- Use structured, visual responses — Cowork renders markdown well

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
| onboarding | `/tilppa-agents:tilppa-onboarding` | New team member onboarding |
| governance | `/tilppa-agents:tilppa-governance` | Clearance, shaping, trust, policies |
| skills | `/tilppa-agents:tilppa-skills` | Load org-specific custom skills |
| org | `/tilppa-agents:tilppa-org` | Multi-org management, context switching |
| teach | `/tilppa-agents:tilppa-teach` | Analyze codebases, generate knowledge |

## Knowledge-First Rule

**ALWAYS check the knowledge database BEFORE reading source code.**

1. `/tilppa-agents:tilppa-knowledge` -- search for relevant knowledge
2. `/tilppa-agents:tilppa-decision` -- search for related decisions
3. Only then read source code if knowledge does not cover the topic

- **Knowledge** = current state (how things work now)
- **Decisions** = historical record (what was decided and why)

## Task Execution Rule

**ALL TP-tasks MUST be executed through a workshop:**

1. `tasks_get id="TP-X"` — read task description (contains all context, references, and instructions)
2. `workshop_start topic="TP-X: [title]" template_name="quick|default" roles=[...]`
3. Follow runbook phases — task description is the single source of truth
4. `workshop_end summary="..."` + `tasks_update id="TP-X" status="done"`

**Never execute tasks directly without a workshop wrapper.** Workshops provide: audit trail, phase discipline, knowledge impact scan, and session continuity (new session can resume from workshop ID).

**Phase progression (TP-865):**

After `workshop_start`, call `runbook_get name=<template>` once, then use `TodoWrite` to create one entry per phase (content = phase_instructions). Drive progress through the list. End with one `workshop_save_output` blob + `workshop_end`. Skips `workshop_advance` entirely. This is the default — use it unless you specifically need server-side phase synchronisation. Legacy `workshop_advance` / per-phase save / `workshop_synthesize` endpoints deprecate in TP-865 part 2 (after TP-771).

**Minimal task prompt format** (everything else comes from task description):
```
tasks_get id="TP-X"
workshop_start topic="TP-X: [title]" template_name="quick" roles=[...]
```

## Rules

**NEVER:**
- Skip the knowledge-first check before reading code
- Assume infrastructure details -- verify with knowledge DB first
- Shorten UUIDs -- always use full IDs for knowledge, decisions, tasks, and sessions
- Respond without knowing the user's profile. If refresh hasn't been run, call `whoami` first. Adapt communication based on the user's context field: match vocabulary, detail level, and language to their role and expertise.

**ALWAYS:**
- Use `TodoWrite` to track multi-step work
- Use `AskUserQuestion` before starting complex tasks
- Adapt communication style to the user (check `whoami` and contacts)
- Use `present_files` when producing documents, exports, or deliverables

## Git Commits

All commit messages MUST be in English.

## Tag Convention

All tags follow `prefix:value` format: `scope:`, `project:`, `feature:`, `epic:`, `version:`, `style:`, `skills:`, `category:`, `jira:`, `pr:`, `sprint:`, `system:`

## MCP Servers

2 MCP servers connect automatically via OAuth:
- `tilppa-agents` -- core: tasks, knowledge, decisions, workshops, contacts, teach, governance (trust, autonomy, approval gates)
- `tilppa-admin` -- settings, onboarding, audit, clearance, policies, shaping

Tools are discovered on-demand via ToolSearch instead of loading all into context.

## Authentication

This plugin uses **OAuth 2.1** via WorkOS AuthKit. On first MCP tool call, a browser opens for login. Tokens are managed automatically.

## Troubleshooting

If MCP tools are missing or `/tilppa-agents:tilppa-refresh` fails:

1. **Plugin enabled?** -- Check tilppa-agents plugin is active in Cowork settings
2. **Authenticated?** -- Check connection status, re-authenticate if needed
3. **Server running?** -- Check https://agents.tilppa.com/health
4. **Token expired?** -- Re-authenticate to refresh
5. **Wrong org?** -- Use `/tilppa-agents:tilppa-org` to switch organization
6. **Reinstall plugin** -- Remove and reinstall the plugin in Cowork
