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

### Documents (unlimited content)

For long-form reports, analyses, and digests that exceed knowledge item limits — and for long-lived documents that evolve over time (per-idea specs, master indexes, run reports).

| Tool | Purpose |
|------|---------|
| `documents_create` | Create document. `type` required (report\|analysis\|brief\|digest\|note). No content limit. `items` array for batch (max 50). `metadata` JSONB for { date, source_signals[], audience, runbook_id }. Optional `knowledge_id` to link to a knowledge item. |
| `documents_get` | Get full content by `id` — no truncation |
| `documents_list` | List by `type`/`tags_any`/`tags_all`/`tag_prefix` — metadata only (no content) |
| `documents_search` | Semantic search on title+description. `search` required. |
| `documents_update` | Partial update — any combination of: `title`, `content`, `description`, `type`, `tags` (replace), `add_tags`, `remove_tags`, `metadata`, `knowledge_id`, `visibility`. `updated_at` auto-set. |
| `documents_delete` | Delete (Admin). `ids` array for batch (max 50). |

```
# Create a long report
documents_create type="digest" title="Iran Daily 7.4.2026" content="[koko raportti]" tags=["project:intelligence"]

# Find all geo digests
documents_list type="digest" tags_any=["scope:geo"]

# Get full content
documents_get id="<uuid>"

# Update tags only (content untouched)
documents_update id="<uuid>" add_tags=["status:validated"]

# Evolve content: read → edit → write back
documents_get id="<uuid>"                          # read current content
documents_update id="<uuid>" content="[existing + new section]"
```

### Update vs Create — use `documents_update` for evolving documents

**Prefer `documents_update` over create+delete** when:
- The document represents a single entity that changes state (idea doc: draft → validated → live)
- You maintain an index that gets new rows (master index of approved/rejected ideas)
- History continuity matters — a new UUID breaks references from decisions/tasks that link to the old one

**Use `documents_create` (and keep old) only when:**
- The old doc is a frozen snapshot (daily digest 2026-04-16 stays that date forever; tomorrow gets its own)
- You explicitly want two separate documents for comparison

**Never** `documents_delete` + `documents_create` when `documents_update` would do — you lose UUID continuity and audit trail.

### Exporting Knowledge

In Cowork, knowledge search results can be exported as documents:
- Use Cowork's docx/pdf skills to create formatted knowledge reports
- Use `present_files` to share exports with the user

### Related Org Skills

- `skills:tagging` — Full tag convention and search guide
- `skills:knowledge-review` — Weekly review process for knowledge DB quality
- `skills:context-preservation` — When and what to save as knowledge
