---
name: tilppa-governance
description: "Manage information shaping, clearance levels, trust scores, approval gates, and access policies. Use when controlling what users can see, checking permissions, granting temporary access, managing progressive autonomy, or asking who can see what."
argument-hint: "[action: status|check|grant] [details]"
allowed-tools: ToolSearch
---

## Governance Stack

```
Layer 4: INFORMATION SHAPING — Same truth, right shape for each audience
Layer 3: TRUST & AUTONOMY — AI that earns its autonomy
Layer 2: OBSERVABILITY — Automatic action logging
Layer 1: FOUNDATION — Roles, Tasks, Knowledge, Decisions
```

## Tools

### Clearance (server: admin)

| Tool | Purpose |
|------|---------|
| `clearance_my` | Your clearance level + active grants (topic) |
| `clearance_levels_list` | List all clearance levels |
| `clearance_level_set` | Set user's clearance level (user_id, level) |
| `clearance_check` | Check if user can see category (user_id, category, topic) |
| `grant_create` | Grant temporary access |
| `grant_revoke` | Revoke a grant |
| `grants_list` | List clearance grants (user_id, status, limit) |
| `grants_history` | Grant history (user_id, limit) |

### Shaping & Policies (server: admin)

| Tool | Purpose |
|------|---------|
| `shaping_status` | Show what you see per category |
| `content_shape` | Preview shaped output (data_type, topic, content) |
| `policy_check` | Check access for a data type/topic/user |
| `policies_list` | List all policies (include_inactive) |
| `policies_get` | Get policy details (name or id) |
| `policies_create` | Create a policy (admin) |
| `policies_update` | Update policy (id + params) |
| `policies_delete` | Delete non-system policy (id) |
| `audit_list` | View audit log entries |

### Trust (server: agents)

| Tool | Purpose |
|------|---------|
| `trust_score_get` | Get trust score (role, action_type, lookback_days) |
| `trust_scores_list` | List all trust scores (min_actions, lookback_days) |

### Autonomy (server: agents)

| Tool | Purpose |
|------|---------|
| `autonomy_status` | Current autonomy level (role, action_type) |
| `autonomy_list` | List all autonomy configs |

### Approval Gates (server: agents)

| Tool | Purpose |
|------|---------|
| `gates_list` | List approval gates |
| `gates_check` | Check if action requires approval |
| `gates_create` | Create approval gate |
| `gates_update` | Update approval gate |
| `gates_delete` | Delete approval gate |
| `approvals_list` | List pending approvals |
| `approvals_get` | Get approval details |
| `approvals_vote` | Vote on an approval (approve/reject) |

### Roles (server: agents)

| Tool | Purpose |
|------|---------|
| `roles_list` | List all roles with overview (name, category, agent_id, status) |
| `roles_get` | Full role details incl. soul_data. Key params: `role_name`, `agent_id` |
| `role_manage` | CRUD roles + activate/deactivate. Key params: `action` (add/remove/create/update/delete), `role_name`, `agent_id`, `soul_data`, `status` (active/suspended/retired), `tier` (1/2), `autonomy_ceiling` |

### Groups (server: agents)

| Tool | Purpose |
|------|---------|
| `groups_list` | List groups |
| `group_manage` | Create, update, or delete groups |

## Cowork Notes

When displaying governance status or clearance info, present results clearly in text — use `TodoWrite` if the user wants to track multiple governance actions. For policy reports or audit logs, use Cowork's docx/pdf skills and `present_files`.

## Usage Examples

```
shaping_status                    # What you can see
clearance_my                      # Your clearance level
clearance_my topic="financials"   # Clearance for specific topic
policy_check data_type="knowledge" topic="financial-budget" user_id="..."
content_shape data_type="knowledge" topic="budget" content="..."
gates_check action_type="deploy"  # Check if deploy needs approval
approvals_list                    # List pending approvals
roles_list                        # List all roles (overview)
roles_get role_name="PekanBuddy"  # Full role details + soul_data
roles_get agent_id="pekan-buddy"  # Lookup by agent_id
role_manage action="create" role_name="MyAgent" category="tech" description="..." agent_id="my-agent" soul_data={"id":"my-agent","role":"MyAgent","persona":{"goals":["..."]}} status="active"
role_manage action="update" role_name="MyAgent" soul_data={...} status="suspended"
```

## Information Shaping

Agents operate with full information — only OUTPUT is shaped according to clearance level.

**Shape types:** full, directional, summary, abstract, none

## JIT Access (Temporary Grants)

```
grant_create
  user_id: "uuid"
  grant_type: "topic_access"
  topic: "financial-q4"
  reason: "Q4 report preparation"
  valid_hours: 48

grant_revoke grant_id: "grant-uuid" reason: "No longer needed"
```

## Trust Score & Autonomy

| Score | Level | Behavior |
|-------|-------|----------|
| 0.90+ | Excellent | Auto-approve |
| 0.70-0.89 | Good | Auto-approve, full logging |
| 0.50-0.69 | Developing | Human review recommended |
| < 0.50 | Learning | Human approval required |
