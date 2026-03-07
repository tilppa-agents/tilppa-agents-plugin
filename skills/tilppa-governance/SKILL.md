---
name: tilppa-governance
description: "Manage information shaping, clearance levels, trust scores, and access policies. Use when controlling what users can see, checking permissions, granting temporary access, or managing progressive autonomy."
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

### Clearance

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

### Shaping & Policies

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

### Trust

| Tool | Purpose |
|------|---------|
| `trust_score_get` | Get trust score (role, action_type, lookback_days) |
| `trust_scores_list` | List all trust scores (min_actions, lookback_days) |
| `trust_score_history` | Score history (role, action_type, days) |

### Autonomy

| Tool | Purpose |
|------|---------|
| `autonomy_status` | Current autonomy level (role, action_type) |
| `autonomy_list` | List all autonomy configs |
| `autonomy_pending` | List pending escalations |
| `autonomy_approve` | Approve escalation (role, action_type) |
| `autonomy_reject` | Reject escalation (role, action_type) |
| `autonomy_override` | Set autonomy level (role, action_type, level 1-4, reason) |
| `autonomy_downgrade` | Downgrade autonomy (role, action_type, to_level, reason) |
| `autonomy_clear_override` | Clear override (role, action_type) |

## Usage Examples

```
shaping_status                    # What you can see
clearance_my                      # Your clearance level
clearance_my topic="financials"   # Clearance for specific topic
policy_check data_type="knowledge" topic="financial-budget" user_id="..."
content_shape data_type="knowledge" topic="budget" content="..."
```

## Information Shaping

Agents operate with full information — only OUTPUT is shaped according to clearance level.

**Clearance Levels:**

| Level | Score | Sees |
|-------|-------|------|
| founder | 100 | Everything |
| executive | 80 | Nearly everything |
| manager | 60 | Team data |
| employee | 40 | Own work data |
| partner | 20 | Limited data |
| customer | 10 | Public data |

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
