---
name: tilppa-knowledge
description: "Search, create, and manage the Tilppa knowledge base. Use when looking up architecture, features, patterns, or saving new learnings. Always check knowledge BEFORE reading source code."
argument-hint: "[action: search|create|update|get] [topic or query]"
allowed-tools: ToolSearch
---

## Tilppa Knowledge Base

### Knowledge-First Rule (CRITICAL)

**ALWAYS check the knowledge database BEFORE reading source code.**

1. `knowledge_list` — search for relevant knowledge
2. `decisions_list` — search for related decisions
3. ONLY THEN read source code if knowledge doesn't cover the topic

### Tools

| Tool | Purpose |
|------|---------|
| `knowledge_list` | Search knowledge base. `search` for semantic search, `tags_any` for tag filtering, `tags_all` for AND logic, `tag_prefix` for prefix filtering (e.g. "skills"), `role` for agent role filter, `limit` (default 20) |
| `knowledge_create` | Save new knowledge. **`item_overview` required** (1-2 sentence summary, max 200 chars). `items` array for batch (max 50). `status` (draft/published, optional) |
| `knowledge_get` | Get full details. `ids` array for batch retrieval (max 50) |
| `knowledge_update` | Update: content, item_overview, topic, tags, add_tags, remove_tags, visibility |
| `knowledge_delete` | Delete (Admin). `ids` array for batch deletion (max 50) |
| `knowledge_review` | Manage draft items: list, approve, reject, edit. Use `approve_all`/`reject_all` for batch actions. `topic` param available for edit action |

### Search Examples

```
# Semantic search
knowledge_list search="how does the workshop flow work"

# Tag filtering
knowledge_list tags_any=["feature:pricing", "project:tilppa-agents"]

# Topic filtering
knowledge_list topic="tag-system"

# Combined
knowledge_list search="authentication" tags_any=["project:tilppa-frontend"]
```

### Creating Knowledge

Use `AskUserQuestion` to confirm topic, tags, and visibility when the user asks to save knowledge but details are unclear.

```
knowledge_create
  role: "ArchitectAgent"
  topic: "topic-in-kebab-case"
  content: "300-500 words, enough context for standalone use"
  item_overview: "1-2 sentence summary for list views, max 200 chars"
  tags: ["project:tilppa-agents", "feature:workshop-flow", "category:architecture"]
  visibility: "team"
```

### Content Length

**300-500 words per item.** Too short = missing context. Too long = dilutes embedding quality.

### Category Tags

`category:architecture`, `category:feature`, `category:style-guide`, `category:pitfall`, `category:skill`, `category:migration-guide`

### Exporting Knowledge

In Cowork, knowledge search results can be exported as documents:
- Use Cowork's docx/pdf skills to create formatted knowledge reports
- Use `present_files` to share exports with the user

### Related Org Skills

- `skills:tagging` — Full tag convention and search guide
- `skills:knowledge-review` — Weekly review process for knowledge DB quality
- `skills:context-preservation` — When and what to save as knowledge
