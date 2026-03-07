---
name: tilppa-notification
description: "Manage notifications, reminders, and messages between team members. Use when creating reminders, sending messages to colleagues, checking pending notifications, or when someone says 'remind me' or 'tell X that'."
argument-hint: "[action: list|create|read] [details]"
allowed-tools: ToolSearch
---

## Tilppa Notifications

### Query Notifications

```
notifications_query mode: "pending"              # Pending only (default)
notifications_query mode: "list"                  # All notifications
notifications_query mode: "unread_count"          # Count of unread only
notifications_query mode: "pending" recipient: "Otto"       # For specific user
notifications_query mode: "list" unread_only: true          # Unread only
```

Act on pending notifications immediately — read them aloud and ask how to proceed.

### Create a Reminder

When a user says "remind X tomorrow at 12 that...":

```
notifications_create
  recipient: "Otto"
  type: "reminder"
  title: "Short title"
  message: "Detailed information..."
  scheduled_for: "2026-01-26T12:00:00"
```

### Send a Message to a Colleague

```
notifications_create
  recipient: "Pekka"
  type: "message"
  title: "Need your input"
  message: "I drafted the proposal. Could you review it?"
  related_table: "tasks"            # Optional: link to a record
  related_id: "task-uuid"           # Optional: ID of the linked record
```

### Mark as Read

When the user acknowledges a notification ("ok got it", "thanks"):

```
notifications_mark_read id: "notification-uuid"  # Single notification
notifications_mark_read all: true                 # Mark all as read
```

### Types

| Type | Usage |
|------|-------|
| `reminder` | Scheduled reminder for a future time |
| `message` | Direct message to a team member |
| `task_assigned` | Automatic notification when a task is assigned |
