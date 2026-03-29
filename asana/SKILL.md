---
name: asana
description: "Manage Asana tasks, projects, portfolios, and status updates. Use this skill when the user wants to create tasks, check what's overdue, update project status, search across workspaces, or manage any aspect of their Asana workflow."
---

# Asana — Task & Project Management

Manage tasks, projects, portfolios, and status updates in Asana.

## Quick Reference

### My Tasks
```
Get my current tasks:        get_my_tasks
Get a specific task:         get_task(task_id)
Search tasks:                search_tasks_preview(query)
Search any object:           search_objects(query)
```

### Task Operations
```
Create a task:               create_task_preview(name, project_id, ...)
Update a task:               update_tasks(task_id, fields...)
Add a comment:               add_comment(task_id, text)
Delete a task:               delete_task(task_id)
Get attachments:             get_attachments(task_id)
```

### Projects & Portfolios
```
List projects:               get_projects
Get project details:         get_project(project_id)
Create a project:            create_project_preview(name, team_id, ...)
Project status update:       create_project_status_update(project_id, ...)
Get status overview:         get_status_overview(project_id)
Get portfolios:              get_portfolios
Get portfolio items:         get_items_for_portfolio(portfolio_id)
```

### Organization
```
Get my profile:              get_me
List teams:                  get_teams
List users:                  get_users
Get a user:                  get_user(user_id)
```

## Common Workflows

### "What's on my plate?"
1. Call \`get_my_tasks\` to pull all tasks assigned to you
2. Sort by due date — highlight overdue items first
3. Group by project for context
4. Present as a prioritized list: OVERDUE → DUE TODAY → THIS WEEK → LATER

### "What's overdue?"
1. Call \`get_my_tasks\`
2. Filter for tasks where \`due_on\` < today and \`completed\` = false
3. Present with project context and days overdue
4. Offer to update due dates or add comments

### "Create a task"
1. Ask for: task name, project, assignee, due date, description
2. Call \`create_task_preview\` with the provided details
3. Confirm creation with task link

### "Update project status"
1. Call \`get_project\` to get current project details
2. Call \`get_tasks\` for the project to assess completion rate
3. Call \`create_project_status_update\` with:
   - Status color: green (on track), yellow (at risk), red (off track)
   - Summary text covering: progress, blockers, next steps
4. Confirm the update was posted

### "Search for something"
1. Try \`search_tasks_preview(query)\` for task-specific searches
2. Use \`search_objects(query)\` for broader searches (projects, portfolios, etc.)
3. Present results with links and context

### "Portfolio review"
1. Call \`get_portfolios\` to list all portfolios
2. Call \`get_items_for_portfolio\` for the target portfolio
3. For each project, call \`get_status_overview\` to pull latest status
4. Present as a dashboard: project name, status color, owner, last update summary

## CEO Prioritization

When presenting tasks or projects, always prioritize for a CEO audience:

### Task Priority Order
1. **OVERDUE** — Red flag, needs immediate attention or delegation
2. **BLOCKED** — Something is stuck and may need Josh to unblock
3. **DUE TODAY/TOMORROW** — Immediate action items
4. **WAITING ON OTHERS** — Delegated items needing follow-up
5. **THIS WEEK** — Upcoming commitments
6. **LATER** — Can be batched or delegated

### Status Update Tone
- Be direct and factual — CEOs want signal, not noise
- Lead with the status color and one-line summary
- Follow with specifics only if asked
- Always surface blockers — that's where a CEO adds the most value

## Presentation Format

### Single Task
```
📋 [Task Name]
   Project: [Project Name]
   Assignee: [Name] | Due: [Date] | Status: [Complete/Incomplete]
   Notes: [First line of description if relevant]
```

### Task List
```
🔴 OVERDUE (3)
   • [Task] — Project X — 5 days overdue
   • [Task] — Project Y — 2 days overdue
   • [Task] — Project Z — 1 day overdue

🟡 DUE TODAY (2)
   • [Task] — Project A
   • [Task] — Project B

🟢 THIS WEEK (4)
   • [Task] — Project C — Due Wed
   • [Task] — Project D — Due Fri
   ...
```

### Portfolio Dashboard
```
📊 [Portfolio Name]

🟢 Project Alpha — On Track — Last update: Mar 25
   "Phase 2 development complete, moving to QA"

🟡 Project Beta — At Risk — Last update: Mar 22
   "Waiting on vendor contract, 3 day delay expected"

🔴 Project Gamma — Off Track — Last update: Mar 20
   "Key resource on leave, timeline pushed 2 weeks"
```

## Error Handling

- If \`get_my_tasks\` returns empty → confirm the Asana workspace/user is correct
- If a task_id is not found → search by name using \`search_tasks_preview\`
- If project creation fails → verify team_id with \`get_teams\` first
- Always confirm destructive actions (delete, reassign) before executing

## Security Rules

- Never delete tasks without explicit confirmation
- Confirm before reassigning tasks to other people
- Don't modify completed tasks unless specifically asked
- When creating status updates, always show the draft before posting

## See Also

- [daily-meeting-update](../daily-meeting-update/SKILL.md) — Morning briefing includes Asana tasks
- [recipe-weekly-review](../recipe-weekly-review/SKILL.md) — Weekly review pulls Asana data
