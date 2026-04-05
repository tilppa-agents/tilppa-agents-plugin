---
name: tilppa-org
description: "Manage organizations: list, switch context, provision new orgs, and view org hierarchy. Use when working across multiple organizations, setting up new ones, or viewing cross-org portfolio."
argument-hint: "[action: list|switch|info|tree|provision] [org-slug]"
allowed-tools: ToolSearch
---

## Tools

### Organization (server: agents)

| Tool | Purpose |
|------|---------|
| `org_list` | List all organizations |
| `org_info` | Get details for an org |
| `org_tree` | Show org hierarchy |
| `org_switch` | Switch org context |
| `org_set_template` | Set org milestone template |
| `milestones_portfolio` | Cross-org milestone portfolio |

### Organization Management (server: platform — founder/dev only)

| Tool | Purpose |
|------|---------|
| `org_create` | Create a new org |
| `org_provision` | Provision org DB + tables |
| `org_update` | Update org settings |

### Projects (server: agents)

| Tool | Purpose |
|------|---------|
| `project_list` | List projects in current org |
| `project_get` | Get project details |
| `project_set` | Set active project context |
| `project_update` | Update project settings |

## Usage Examples

```
# Switch org context
org_switch slug="kuurahealth"     # Switch to KuuraHealth
org_switch                        # Clear override, return to default

# Browse orgs
org_list
org_info slug="tilppa"
org_tree

# Create and provision (platform-only)
org_create name="New Org" slug="new-org"
org_create name="New Org" slug="new-org" org_type="project" parent_slug="tilppa"
org_provision slug="new-org"

# Projects
project_list
project_set slug="tilppa-agents"
```

## Cowork Notes

When switching orgs, confirm with `AskUserQuestion` if the user hasn't specified which org. After switching, re-run refresh to load the new org context.

## Org Context Priority

Resolution order: `org_switch` override > `TILPPA_ORG` env > `.tilppa-org` file

## Cross-Org Portfolio

```
milestones_portfolio              # Progress across all child orgs
```
