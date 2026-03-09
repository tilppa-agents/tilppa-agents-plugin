---
name: tilppa-workshop
description: "Run multi-agent workshops with structured phases. Use when planning features, making architectural decisions, reviewing work, or solving cross-cutting problems. Guides through setup, framing, exploration, synthesis, decision, and action phases."
argument-hint: "start topic=<topic> [project=<slug>] [roles=<agents>] [task=TP-XXX]"
allowed-tools: ToolSearch
---

## Multi-Agent Workshop

Structured facilitation for planning, design, and decision-making with multiple agent roles.

### Slash Command (for user)

When the user requests a workshop, **always provide the full command** with all parameters:

```
/tilppa-workshop start topic="TP-XXX: Clear description" project=tilppa-agents roles=ArchitectAgent,BackendDevAgent,FrontendDevAgent task=TP-XXX
```

Never provide just `/tilppa-workshop` without parameters.

### MCP Call (internal)

```
workshop_start
  topic: "TP-XXX: Clear description"
  project: "tilppa-agents"
  template_name: "default"
  roles: ["ArchitectAgent", "BackendDevAgent"]
  workshop_type: "planning"
  depth: "standard"
```

### Parameters

| Parameter | Required | Description | Example |
|-----------|----------|-------------|---------|
| `topic` | **yes** | Task ID + description | `"TP-268: Fix Stripe webhook"` |
| `project` | no | Project slug (auto-detected from topic) | `"tilppa-agents"` |
| `template_name` | no | Runbook template (default: `"default"`) | `"quick"`, `"review"` |
| `roles` | no | Participating agents | `["ArchitectAgent", "BackendDevAgent"]` |
| `workshop_type` | no | Workshop type | `"discovery"`, `"planning"`, `"review"` |
| `depth` | no | Analysis depth | `"minimal"`, `"standard"`, `"full"` |

### Runbook Templates

`quick` (bugfix, small change) | `default` (new feature, architecture, 7 phases) | `review` (code review, retro)

### Workshop Phases (default)

1. **Setup** — Load context, knowledge-first
2. **Role Validation** — Call `roles` to load ALL available roles with descriptions, then decide which to add/remove
3. **Framing** — Goal, constraints, success criteria (BusinessAgent leads)
4. **Exploration** — Each role's perspective in parallel
   - **KNOWLEDGE FIRST: Before reading ANY source code**, search `knowledge_list` and `decisions_list` for relevant topics. Then use `knowledge_get` and `decisions_get` to load full item content (list only returns truncated previews). Only read code if knowledge does not cover the question.
5. **Synthesis** — Combine findings, identify conflicts (LeadDevAgent leads)
6. **Decision** — Decision + rationale (ArchitectAgent leads)
7. **Action** — Implementation, tasks, code

**NEVER write code before the Action phase (7/7)!**

### Session Commands

```
workshop_advance      session_id="..."                          # Advance to next phase
workshop_status       session_id="..."                          # Show session state
workshop_end          session_id="..." [summary="..."]          # End and save session
workshop_save_output  session_id="..." phase_id="..." output={...} [role="..."]
workshop_list         [show_recent=true]                        # List recent workshops
```

### Exploration Tools

```
exploration_contribute  session_id="..." role="..." perspective="..." [recommendations/concerns/questions optional]
workshop_synthesize     session_id="..." [phase_id="..."]       # Synthesize outputs
```

### Runbook Management

`runbook_list` | `runbook_get name` | `runbook_create name ...` | `runbook_customize name ...` | `runbook_reset name`

### Role Ordering

BusinessAgent (goal) → ArchitectAgent (design) → LeadDevAgent (plan) → other roles → BusinessAgent (review)

### Wrap-up (NEVER skip — most important phase)

This is the most valuable phase. Skipping it loses all learnings for the team.

1. **Milestones** — `milestones_update requirement_key="..." status="completed|in_progress"`
2. **Tasks** — verify all task statuses are correct (`done`, `in_progress`, `blocked`)
3. **Knowledge** — ALWAYS search first, then update or create:
   - `knowledge_list search="topic"` — find existing items
   - If found → `knowledge_update id="..." content="..."` (update existing)
   - If NOT found → `knowledge_create` (create new)
   - **NEVER create without searching first — duplicates degrade the knowledge base**
4. **Decisions** — same dedup rule (if already created during decision phase, do not duplicate)

### Related Org Skills

Load these before starting a workshop: `knowledge_list tag_prefix="skills"`

- `skills:workshop-flow` — Workshop best practices, phase discipline, knowledge-first rule
- `skills:context-preservation` — What/when/how to save learnings during and after workshop
- `skills:tagging` — Tag conventions for knowledge, decisions, and tasks created during workshop
