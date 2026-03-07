---
name: tilppa-teach
description: "Analyze codebases, files, git history, and dependency graphs to generate knowledge items. Use when onboarding to a new codebase, documenting existing code, or extracting patterns from repositories."
argument-hint: "[mode: feature|module|codebase|commits|graph] [details]"
allowed-tools: ToolSearch
---

## Knowledge Teach

Analyze source code and generate draft knowledge items for review.

### Modes

| Mode | Description | Parameters |
|------|-------------|------------|
| `feature` | Single feature | `paths` (files/dirs/globs) |
| `module` | Code module | `paths` |
| `overview` | Project overview | `paths` |
| `commits` | Git history → themes | `range` (`last:N`, `since:YYYY-MM-DD`), `filter` (`significant` default, `all`) |
| `codebase` | Auto-discover entire project | no paths required |
| `graph` | Dependency graph | `paths` (optional) |

### Examples

```
# Full codebase analysis
knowledge_teach mode="codebase" feature_name="tilppa-agents" project="tilppa-agents"

# Specific feature with description
knowledge_teach mode="feature" feature_name="tag-system" paths=["src/utils/tags.ts"] project="tilppa-agents" description="Tag parsing and validation utilities"

# Git history (significant commits only, default)
knowledge_teach mode="commits" feature_name="recent" range="last:20" project="tilppa-agents"

# Git history (all commits including minor)
knowledge_teach mode="commits" feature_name="recent" range="last:50" project="tilppa-agents" filter="all"

# Dependency graph
knowledge_teach mode="graph" feature_name="services" paths=["src/services/"] project="tilppa-agents"
```

### Depth Levels

| Level | Description |
|-------|-------------|
| `architecture` | Imports/exports/declarations (default) |
| `detailed` | Small files in full |
| `full` | All content included |

### Output

Creates DRAFT knowledge items for user review via `knowledge_review action="list"`.
