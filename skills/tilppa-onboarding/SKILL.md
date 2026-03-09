---
name: tilppa-onboarding
description: "Guide new team members through Tilppa onboarding. Runs a 2-question interview to determine role type and first goal, activates relevant agent roles, and saves profile. Use when a new user joins the team or when setting up a new team member."
argument-hint: "[user-name]"
allowed-tools: ToolSearch
---

## Tilppa Onboarding (2-Question Flow)

### Language

Detect from user's environment locale. Supported: fi, en. Default: en.

### Flow

1. **Start:** `onboarding_update status="in_progress"` (server: admin)
2. **Question 1 — Role type:** "What best describes your role?"
   - `developer` — technical, writes code
   - `product` — product management, requirements, roadmap
   - `designer` — UI/UX design, visual design
   - `other` — none of the above
3. **Question 2 — First goal:** "What do you want to do first?"
   - `tasks` — manage and track work
   - `knowledge` — search and document learnings
   - `workshop` — plan features or decisions collaboratively
   - `other` — explore on my own
4. **Save answers:** `onboarding_update answers={"role_type": "...", "first_goal": "..."}` (server: admin)
5. **Save profile:** `users_manage action="update_profile" user_id="..." profile={...}` (server: platform)
6. **Activate roles:** `role_manage action="add" role_names=[...]` (server: agents)
7. **Complete:** `onboarding_update status="completed"` (server: admin)

### Role Activation Mapping

| role_type | Activated roles |
|-----------|----------------|
| `developer` | LeadDevAgent, ArchitectAgent, BackendDevAgent |
| `product` | ProductAgent, BusinessAgent |
| `designer` | UIDesignerAgent, ProductAgent |
| `other` | ProductAgent, BusinessAgent |

### Tools

| Tool | Server | Purpose |
|------|--------|---------|
| `onboarding_get` | admin | Get own onboarding details |
| `onboarding_list` | admin | List all sessions (Admin) |
| `onboarding_update` | admin | Update status or answers |
| `onboarding_create` | admin | Create manually (Admin) |
| `users_manage` | platform | Update user profile (founder/dev only) |
| `role_manage` | agents | Add/remove agent roles |
