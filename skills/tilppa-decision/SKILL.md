---
name: tilppa-decision
description: "Record and search architectural and business decisions. Use when documenting what was decided and why, when looking up past decisions for context, or when a workshop concludes with actionable outcomes."
argument-hint: "[action: search|create|update|get] [topic or query]"
allowed-tools: ToolSearch
---

## Tilppa Decisions

### Tools

| Tool | Description |
|------|-------------|
| `decisions_list` | Search decisions. `search` for semantic search, `tags_any` for filtering, `tags_all` for AND logic, `tag_prefix` for prefix filtering, `role` for agent role filter, `limit` (default 20), `related_entity` for entity filtering |
| `decisions_create` | Save a decision. **`item_overview` required** (1-2 sentence summary, max 200 chars). `items` array for batch (max 50). `visibility` (private/team/public), `related_entity` |
| `decisions_get` | Fetch full details. `ids` array for batch retrieval (max 50) |
| `decisions_update` | Update: decision, item_overview, context, rationale, tags, `add_tags`, `remove_tags`, `visibility`, `related_entity` |
| `decisions_delete` | Delete (Admin). `ids` array for batch deletion (max 50) |

### Search

```
# Semantic search
decisions_list search="why was Tailwind chosen"

# Tag filtering
decisions_list tags_any=["feature:milestones", "epic:TP-41"]
```

### Creation

```
decisions_create
  role: "ArchitectAgent"
  decision: "Clear decision statement"
  item_overview: "1-2 sentence summary for list views, max 200 chars"
  context: "Situation and alternatives considered"
  rationale: "Why this option was chosen"
  tags: ["project:tilppa-agents", "feature:workshop", "epic:TP-164"]
```

### Important Notes

- Always include `context` and `rationale` — a decision without reasoning loses value over time.
- Tag consistently using `project:`, `feature:`, `epic:` prefixes.
- After workshops, record key outcomes as decisions with the workshop session ID in the context.

### Related Org Skills

- `skills:tagging` — Tag convention and search guide
- `skills:context-preservation` — When and what to save as decisions
