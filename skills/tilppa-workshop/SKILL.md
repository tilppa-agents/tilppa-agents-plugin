---
name: tilppa-workshop
description: "Run multi-agent workshops with configurable phases. Use when planning features, making architectural decisions, reviewing work, or solving cross-cutting problems. Phases are dynamic and delivered by MCP server."
argument-hint: "start topic=<topic> [project=<slug>] [roles=<agents>] [task=TP-XXX]"
allowed-tools: ToolSearch
---

## Multi-Agent Workshop

Structured facilitation for planning, design, and decision-making with multiple agent roles.

When the user requests a workshop, **always provide the full command** with all parameters:

```
/tilppa-workshop start topic="TP-XXX: Clear description" project=tilppa-agents roles=ArchitectAgent,BackendDevAgent task=TP-XXX
```

### Parameters

| Parameter | Required | Description | Example |
|-----------|----------|-------------|---------|
| `topic` | **yes** | Task ID + description | `"TP-268: Fix Stripe webhook"` |
| `project` | no | Project slug (auto-detected from topic) | `"tilppa-agents"` |
| `template_name` | no | Runbook template (default: `"default"`) | `"quick"`, `"pr-review"` |
| `roles` | no | Participating agents | `["ArchitectAgent", "BackendDevAgent"]` |
| `workshop_type` | no | Workshop type | `"discovery"`, `"planning"`, `"review"` |
| `depth` | no | Analysis depth | `"minimal"`, `"standard"`, `"full"` |

### Runbook Templates

`quick` (bugfix, small change) | `default` (new feature, architecture) | `pr-review` (PR code review) | `review` (work/decision review)

### Phase Instructions Cascade

Phase instructions are delivered by MCP server via `workshop_advance`. Three layers merge in order:

1. **Template** (base) — runbook template phases from DB
2. **Project** — `project_update id="slug" config={"phase_overrides": {"modify_phases": {"exploration": {"instructions": "Custom"}}}}`
3. **User** — `runbook_customize name phase_overrides={...}`

Merge: `modify_phases` = shallow merge per phase, `remove_phases` = filter, `add_phases` = append.

**Follow `phase_instructions` in each phase** — they contain goals, instructions, expected outputs, and thinking mode.

### Session Commands

```
workshop_start        topic="..." [project/template_name/roles/workshop_type/depth]
workshop_advance      session_id="..."                          # Advance to next phase
workshop_status       session_id="..."                          # Show session state
workshop_end          session_id="..." [summary="..."]          # End and save session
workshop_save_output  session_id="..." phase_id="..." output={...} [role="..."]
workshop_list         [show_recent=true]                        # List recent workshops
exploration_contribute session_id="..." role="..." perspective="..." [recommendations/concerns/questions]
workshop_synthesize   session_id="..." [phase_id="..."]         # Synthesize outputs
```

### Runbook Management

`runbook_list` | `runbook_get name` | `runbook_create name ...` | `runbook_update name ...` | `runbook_delete name` | `runbook_customize name ...` | `runbook_reset name`

### Related Org Skills

Load before starting: `knowledge_list tag_prefix="skills"`

- `skills:workshop-flow` — Best practices, phase discipline, knowledge-first rule
- `skills:context-preservation` — What/when/how to save learnings
- `skills:tagging` — Tag conventions for knowledge, decisions, and tasks
