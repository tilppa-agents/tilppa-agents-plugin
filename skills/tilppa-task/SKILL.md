---
name: tilppa-task
description: "Manage Tilppa tasks: create, update, search, list, and batch operations. Use when the user wants to work with tasks, assign work, track progress, or check what needs doing."
argument-hint: "[action: list|create|update|get|delete] [details]"
allowed-tools: ToolSearch
---

## Tilppa Task Management

### Tools

| Tool | Purpose |
|------|---------|
| `tasks_list` | Search tasks. `search` for semantic search, or filter by status/assignee/priority |
| `tasks_create` | Create a task (auto-notifies assignee). `items` array for batch creation (max 50). `related_entity` |
| `tasks_update` | Update task: title, status, assignee, priority, description, tags, add_tags, remove_tags, `related_entity` |
| `tasks_get` | Get task details. `ids` array for batch retrieval (max 50) |
| `tasks_delete` | Delete task permanently (Admin). `ids` array for batch deletion (max 50) |

### Search Examples

```
# Semantic search (natural language)
tasks_list search="authentication and login flow"

# Filtering
tasks_list status="todo" assignee="Pekka" priority="high"

# Combined
tasks_list search="checkout flow" status="in_progress"
```

### Creating a Task

```
tasks_create
  title: "Title (imperative form)"
  description: "Description, context, acceptance criteria"
  priority: "high"
  assigned_to: "Pekka"
  tags: ["project:tilppa-agents", "epic:TP-41", "feature:workshop"]
```

### Batch Creation

```
tasks_create items: [
  { title: "Task 1", priority: "high", tags: ["epic:TP-41"] },
  { title: "Task 2", priority: "medium", assigned_to: "Otto" }
]
```

### Status Flow

`todo` → `in_progress` → `done` (also: `blocked`)

### Related Org Skills

- `skills:tagging` — Tag convention for `project:`, `feature:`, `epic:` prefixes
