---
name: tilppa-task
description: "Manage Tilppa tasks: create, update, search, list, and batch operations. Use when the user wants to work with tasks, assign work, track progress, or check what needs doing."
argument-hint: "[action: list|create|update|get|delete] [details]"
allowed-tools: ToolSearch
---

## Tilppa Task Management

When listing or searching tasks, use `TodoWrite` to display results as a trackable list for the user.

### Tools

| Tool | Purpose |
|------|---------|
| `tasks_list` | Search tasks. `search` for semantic/exact search, filter by status/assignee/priority/tags |
| `tasks_create` | Create a task (auto-notifies assignee). **`item_overview` required** (1-2 sentence summary, max 200 chars). `items` array for batch (max 50) |
| `tasks_update` | Update task fields including `external_id` for external system linking |
| `tasks_get` | Get task details by TP-xxx, UUID, or external ID. `ids` array for batch (max 50) |
| `tasks_delete` | Delete task permanently (Admin). `ids` array for batch (max 50) |

### Search Examples

```
# Semantic search (natural language)
tasks_list search="authentication and login flow"

# Exact identifier match (auto-detected for patterns like DA-2158, JIRA-123)
tasks_list search="DA-2158"

# Filter by status/assignee/priority
tasks_list status="todo" assigned_to="Pekka" priority="high"

# Tag filters (same as knowledge_list/decisions_list)
tasks_list tags_any=["project:tilppa-agents", "project:kuura"]
tasks_list tags_all=["scope:backend", "feature:auth"]
tasks_list tag_prefix="epic"

# Combined: semantic search + tag filter
tasks_list search="checkout flow" status="in_progress" tags_any=["project:tilppa"]
```

### Creating a Task

Before creating, use `AskUserQuestion` to confirm details if the user's request is ambiguous (priority, assignee, tags).

```
tasks_create
  title: "Title (imperative form)"
  description: "Description, context, acceptance criteria"
  item_overview: "1-2 sentence summary for list views, max 200 chars"
  priority: "high"
  assigned_to: "Pekka"
  external_id: "DA-2158"
  tags: ["project:tilppa-agents", "epic:TP-41", "feature:workshop"]
```

### External ID

Link tasks to external systems (Jira, GitHub, etc.). Must be unique per org.

```
# Set on create
tasks_create title="Fix login bug" external_id="DA-2158"

# Set on update
tasks_update id="TP-113" external_id="DA-2158"

# Lookup by external ID
tasks_get id="DA-2158"
```

### Batch Creation

```
tasks_create items: [
  { title: "Task 1", item_overview: "Summary of task 1", priority: "high", external_id: "DA-100", tags: ["epic:TP-41"] },
  { title: "Task 2", item_overview: "Summary of task 2", priority: "medium", assigned_to: "Otto" }
]
```

### Epic-Subtask Linking

When creating subtasks under an epic, **always link them during creation**:

1. Set `related_entity` to the epic task ID on each subtask
2. Update the epic description to list all subtasks with TP-IDs

```
# Subtask with epic link
tasks_create title="Implement Redis model" related_entity="TP-564" tags=["epic:consciousness-lab"]

# Epic description should list subtasks
tasks_update id="TP-564" description="## EPIC: ...\n\n### Subtasks\n- TP-573: Redis model\n- TP-574: CRUD service"
```

Do this during batch creation, not as a separate step afterwards.

### Status Flow

`todo` | `on_hold` | `in_progress` | `in_review` | `blocked` | `done` | `cancelled`

### Contacts

| Tool | Purpose |
|------|---------|
| `contacts_list` | List contacts (filter by type, search) |
| `contacts_get` | Get contact details |
| `contacts_search` | Search contacts by name/email |
| `contacts_create` | Create a contact (name, email, type, notes) |
| `contacts_update` | Update contact details |

### Related Org Skills

- `skills:tagging` — Tag convention for `project:`, `feature:`, `epic:` prefixes
