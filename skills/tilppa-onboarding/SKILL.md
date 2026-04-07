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

Use `AskUserQuestion` for each question step — this gives the user a clean selection UI in Cowork.

1. **Start:** `onboarding_update status="in_progress"` (server: admin)
2. **Question 1 — Role type:** Use `AskUserQuestion` with header "Role" and options:
   - Developer — koodaus, arkkitehtuuri, integraatiot
   - Product — tuotehallinta, roadmap, priorisointi
   - Designer — UI/UX, visuaalinen suunnittelu
   - Other — custom role description
3. **Question 2 — First goal:** Use `AskUserQuestion` with header "First goal" and options:
   - Tasks — tehtävien hallinta ja seuranta
   - Knowledge — tietokannan selaaminen ja oppiminen
   - Workshop — suunnittelu ja päätöksenteko
   - Other — explore on my own
4. **Save answers:** `onboarding_update answers={"role_type": "...", "first_goal": "..."}` (server: admin)
5. **Step 3 — Generate context:** Using the user's role_type, first_goal, and org_info, write a ~100 word description covering:
   - Their role in the company (real role, not Tilppa agent roles)
   - Their expertise and technical background
   - Communication preferences (language, detail level, vocabulary)
   - Ask the user: "Does this sound right? What would you change?"
   - User approves or edits → save as `answers.context`
6. **Save answers with context:** `onboarding_update answers={"role_type": "...", "first_goal": "...", "context": "...", "locale": "..."}` (server: admin)
7. **Save profile:** `users_manage action="update_profile" user_id="..." profile={...}` (server: platform)
8. **Activate roles:** `role_manage action="add" role_names=[...]` (server: agents)
9. **Complete:** `onboarding_update status="completed"` (server: admin)

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
