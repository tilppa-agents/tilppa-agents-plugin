---
name: tilppa-skills
description: "Load organization-specific custom skills from the knowledge database. Use whenever the user asks for org workflows, patterns, or custom guides. Always check available org skills before building custom solutions."
argument-hint: "[skill-name or 'list']"
allowed-tools: ToolSearch
---

## Org Skills (Meta-Skill)

This is a meta-skill. It loads org-specific custom skills stored as knowledge items tagged with `skills:*`. Unlike built-in skills (these SKILL.md files), org skills are managed per-org via the knowledge database.

### How It Works

1. User runs `list` → skill calls `knowledge_list tag_prefix="skills"` → lists all `skills:*` items
2. User runs a specific skill name → skill calls `knowledge_list topic="skill-name"` or `knowledge_get id="..."` → fetches content
3. Claude reads the content and follows its instructions

### List Available Skills

```
knowledge_list tag_prefix="skills"
```

Returns all org skills with topics and tags.

### Load a Specific Skill

```
# By topic name
knowledge_list topic="skill-name"

# By ID
knowledge_get id="skill-uuid"
```

### Create an Org Skill

```
knowledge_create
  role: "system"
  topic: "skill-topic-kebab-case"
  tags: ["skills:name", "scope:system", "category:skill"]
  visibility: "team"
  content: |
    ## Title (skills:name)

    ### Section 1
    - Clear instruction in imperative form
    - Concrete example

    ### Section 2
    - Additional instructions
```

### Requirements

| Field | Value | Description |
|-------|-------|-------------|
| `role` | `system` | **Required.** Skills are system-level knowledge |
| `topic` | descriptive-name | **Required.** Kebab-case |
| `tags` | `["skills:name", "scope:system", "category:skill"]` | **Required.** `skills:` prefix mandatory |
| `visibility` | `team` or `public` | `team` = org-level, `public` = all orgs |

### Content Guidelines

1. Under 100 lines per skill
2. Use imperative form — "Use QueryBuilder" (not "You can use...")
3. Include concrete examples — code, commands, tables
4. Use one term per concept consistently

### Built-in vs Org Skills

| Built-in Skills | Org Skills (this skill) |
|-----------------|------------------------|
| In repo `.claude/skills/` | In knowledge DB |
| Same for all orgs | Org-specific |
| Updated via `git pull` | Updated via `knowledge_update` |
| MCP tool usage guides | Org workflows and patterns |

### Related Org Skills

- `skills:skill-creation` — Detailed guide for creating org skills (fields, principles, checklist)
- `skills:skill-md-authoring` — Best practices for writing SKILL.md files (TP-296 research)
