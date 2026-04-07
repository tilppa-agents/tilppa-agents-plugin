---
name: tilppa-refresh
description: "Start or refresh a Tilppa session. Loads full context: user info, notifications, active roles, open tasks, knowledge index, and recent decisions. Use at the beginning of every conversation or when context feels stale."
argument-hint: "[depth: minimal|standard|full]"
allowed-tools: ToolSearch
---

## Tilppa Session Refresh

Load the current session context by calling `mcp__tilppa-agents__refresh_tilppa` with depth from `$ARGUMENTS` (default: `standard`).

### Steps

1. Call `mcp__tilppa-agents__refresh_tilppa` with the requested depth
2. Show pending notifications first
3. If user has open tasks, use `TodoWrite` to create a task overview so they can see what's pending
4. If user hasn't stated what they want to work on, use `AskUserQuestion` to ask

### Loaded Context

| Data | Contents |
|------|----------|
| User | Name, role, clearance level |
| Notifications | Pending alerts (show first!) |
| Roles | Active agents by category |
| Tasks | Open tasks, prioritized |
| Knowledge | Topics and tag cloud |
| Decisions | Most recent decisions |
| Contacts | Team and contacts summary |
| Skills | Available org-specific skills |
| Onboarding | Pending onboarding prompt |
| Org | Active org name and slug |

### Depth Levels

| Level | Use case |
|-------|----------|
| `minimal` | Roles + notifications only |
| `standard` | Default — everything above |
| `full` | Extended limits for deep analysis |

### Lightweight Alternative

For notifications only:
1. `mcp__tilppa-agents__whoami`
2. `mcp__tilppa-agents__notifications_query` mode: "pending"
