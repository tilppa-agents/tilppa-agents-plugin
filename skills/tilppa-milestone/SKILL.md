---
name: tilppa-milestone
description: "Track project milestones, manage templates, and view weighted progress. Use for progress tracking, template management, end of work sessions, and cross-org portfolio views."
argument-hint: "[action: list|update|summary|portfolio|template] [details]"
allowed-tools: ToolSearch
---

## Tools

| Tool | Purpose |
|------|---------|
| `milestones_list` | List milestones for a template |
| `milestones_update` | Update milestone status |
| `milestones_summary` | Quick progress overview |
| `milestones_portfolio` | Cross-org portfolio view |
| `org_set_template` | Manage org template assignments |
| `template_manage` | CRUD operations on templates (admin) |

## Usage Examples

```
# List milestones
milestones_list                                  # Primary template
milestones_list template_name="saas-launch"      # Specific template
milestones_list level="all"                      # All levels

# Update progress
milestones_update requirement_key="..." status="completed"
milestones_update requirement_key="..." status="completed" notes="Deployed v2"

# View progress
milestones_summary                       # Primary template
milestones_summary template_name="all"   # Weighted overall view
```

## Org Template Management

```
org_set_template action="list"
org_set_template action="add" template_name="saas-launch" weight=0.5 priority=50
org_set_template action="remove" template_name="saas-launch"
org_set_template action="set_primary" template_name="automation"
org_set_template action="update" template_name="saas-launch" weight=0.7
```

## Template CRUD (Admin)

```
template_manage action="list"
template_manage action="create" name="custom" display_name="Custom" milestones=[...]
template_manage action="add_milestone" name="automation" milestone={...}
template_manage action="remove_milestone" name="automation" requirement_key="..."
template_manage action="delete" name="custom"
```

## Weighted Progress

`overall = sum(template_weight * template_pct) / sum(template_weight)`

## Cowork Notes

When showing milestone summaries, use `TodoWrite` to display progress as a trackable checklist. For portfolio views across organizations, consider exporting as a formatted document using Cowork's xlsx/docx skills and `present_files`.

## Important Notes

- **End of session:** ALWAYS run `milestones_update requirement_key="..." status="completed"` for any milestones achieved during the session.
