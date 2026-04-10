---
name: tilppa-workshop
description: "Run multi-agent workshops with configurable phases. Use when planning features, making architectural decisions, reviewing work, or solving cross-cutting problems. Phases are dynamic and delivered by MCP server."
argument-hint: "start topic=<topic> [project=<slug>] [roles=<agents>] [task=TP-XXX]"
allowed-tools: ToolSearch
---

## Multi-Agent Workshop

Structured facilitation for planning, design, and decision-making with multiple agent roles.

When the user requests a workshop, use `AskUserQuestion` to confirm topic, scope, and relevant agents before starting. Then use `TodoWrite` to track workshop phases as tasks.

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

| Template | Display Name | Use when |
|----------|-------------|----------|
| `quick` | Decision | Bugfix, small change, clear-cut decision |
| `default` | Full Workshop | New feature, architecture, planning + implementation |
| `implementation` | Build | Feature implementation, scope is clear — skip synthesis/decision |
| `pr-review` | PR Review | PR code review |
| `review` | Work Review | Work/decision review, retrospective |

### Phase Instructions Cascade

Phase instructions are delivered by MCP server via `workshop_advance`. Four layers merge in order:

1. **Template** (base) — runbook template phases from DB
2. **Org** — `runbook_org_customize modify_phases={...}` (admin-only, instructions **appended** not replaced)
3. **Project** — `project_update id="slug" config={"phase_overrides": {"modify_phases": {"exploration": {"instructions": "Custom"}}}}`
4. **User** — `runbook_customize name phase_overrides={...}`

Merge: `modify_phases` = shallow merge per phase, `remove_phases` = filter, `add_phases` = append.

**Org layer is special:** instructions are concatenated (appended), not replaced — org rules are mandatory additions that project/user overrides cannot remove. Other fields (goal, depth, etc.) use standard override. Omit `template_name` for global override (all templates).

**Follow `phase_instructions` in each phase** — they contain goals, instructions, expected outputs, and thinking mode.

### Phase Gates

Phases can have exit gates that control when you can leave them:

- **auto** (default) — advances immediately
- **approval_required** — blocks until human approves (stub: use `force=true` to bypass)
- **iterative** — phase repeats up to `max_iterations` times (review-fix-review cycle)

For iterative phases: `workshop_advance iterate=true` re-enters the same phase. `force=true` bypasses any gate. Output keys use `_iter{N}` suffix (e.g., `review_iter1`).

Active iterative phases: pr-review.review(3), implementation.verification(2), review.review(2).

### Session Commands

```
workshop_start        topic="..." [project/template_name/roles/workshop_type/depth]
workshop_advance      session_id="..." [iterate=true] [force=true]  # Advance or iterate
workshop_status       session_id="..."                          # Show session state
workshop_end          session_id="..." [summary="..."]          # End and save session
workshop_save_output  session_id="..." phase_id="..." output={...} [role="..."]
workshop_list         [show_recent=true]                        # List recent workshops
exploration_contribute session_id="..." role="..." perspective="..." [recommendations/concerns/questions]
workshop_synthesize   session_id="..." [phase_id="..."]         # Synthesize outputs
```

### Workshop Output as Documents

In Cowork, workshop results can be exported as documents:
- Use Cowork's docx/pptx/pdf skills to create deliverables from workshop outputs
- Use `present_files` to share the finished documents with the user
- Save outputs to the mounted workspace folder

### Runbook Management

`runbook_list` | `runbook_get name` | `runbook_create name ...` | `runbook_update name ...` | `runbook_delete name` | `runbook_customize name ...` | `runbook_reset name` | `runbook_org_customize [template_name] ...` | `runbook_org_reset [template_name]`

### Related Org Skills

Load before starting: `knowledge_list tag_prefix="skills"`

- `skills:workshop-flow` — Best practices, phase discipline, knowledge-first rule
- `skills:context-preservation` — What/when/how to save learnings
- `skills:tagging` — Tag conventions for knowledge, decisions, and tasks
