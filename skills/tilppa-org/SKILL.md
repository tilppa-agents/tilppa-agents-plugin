---
name: tilppa-org
description: "Manage organizations: list, switch context, provision new orgs, and view org hierarchy. Use when working across multiple organizations, setting up new ones, or viewing cross-org portfolio."
argument-hint: "[action: list|switch|info|tree|provision] [org-slug]"
allowed-tools: ToolSearch
---

## Tools

| Tool | Purpose |
|------|---------|
| `org_list` | List all organizations |
| `org_info` | Get details for an org |
| `org_tree` | Show org hierarchy |
| `org_create` | Create a new org |
| `org_provision` | Provision org DB + tables |
| `org_switch` | Switch org context |
| `org_update` | Update org settings |
| `milestones_portfolio` | Cross-org milestone portfolio |

## Usage Examples

```
# Switch org context
org_switch slug="kuurahealth"     # Switch to KuuraHealth
org_switch                        # Clear override, return to default

# Browse orgs
org_list
org_info slug="tilppa"
org_tree

# Create and provision
org_create name="New Org" slug="new-org"
org_create name="New Org" slug="new-org" org_type="project" parent_slug="tilppa"
org_provision slug="new-org"

# Update org settings
org_update slug="new-org" name="Updated Name" org_type="project"
```

## Org Context Priority

Resolution order: `org_switch` override > `TILPPA_ORG` env > `.tilppa-org` file

## Cross-Org Portfolio

```
milestones_portfolio              # Progress across all child orgs
```
