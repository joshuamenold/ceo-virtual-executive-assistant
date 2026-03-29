---
name: recipe-weekly-review
description: "Run a structured end-of-week review that pulls calendar events, Asana tasks, email threads, and Obsidian notes into a comprehensive weekly review document. Use this skill when the user says 'weekly review', 'end of week review', 'what happened this week', 'week in review', 'wrap up the week', or wants a summary of their week across all systems."
---

# Weekly Review — End-of-Week CEO Briefing

Compile a structured weekly review pulling data from Calendar, Asana, Gmail, and Obsidian.

## When to Use

- Friday afternoon or weekend reflection
- User says "weekly review," "wrap up the week," "what happened this week"
- Preparing for the following week's priorities

## Workflow

### Step 1: Gather Data (run in parallel where possible)

#### Calendar — What happened this week
\`\`\`bash
# Get all events from Monday through today
gws calendar events list --calendar primary --min-time "YYYY-MM-DDT00:00:00Z" --max-time "YYYY-MM-DDT23:59:59Z"
\`\`\`
Or use the Google Calendar MCP:
- \`gcal_list_events\` for this week (Monday → Friday)
- Count meetings, total hours in meetings
- Note any cancellations or rescheduled items

#### Asana — Task progress
- \`get_my_tasks\` — filter for tasks completed this week and tasks still open
- \`get_portfolios\` + \`get_status_overview\` for each active project
- Count: tasks completed, tasks added, tasks overdue

#### Gmail — Communication load
\`\`\`bash
# Emails received this week
gws gmail messages list --query "after:YYYY/MM/DD before:YYYY/MM/DD"
# Emails sent this week
gws gmail messages list --query "from:me after:YYYY/MM/DD before:YYYY/MM/DD"
# Threads still needing reply
gws gmail messages list --query "is:unread in:inbox"
\`\`\`
Or use the Gmail MCP:
- \`gmail_search_messages\` with date-bounded queries
- Count sent vs received
- Flag threads still awaiting reply

#### Obsidian — Notes and decisions
\`\`\`bash
# Notes created or modified this week
find "$OBSIDIAN_VAULT" -name "*.md" -newer /tmp/week-start-marker -type f
# Daily notes from this week
cat "$OBSIDIAN_VAULT/Daily Notes/YYYY-MM-DD.md"  # for each day
\`\`\`
- Pull action items from daily notes
- Surface any decision logs created this week
- Check for incomplete action items carried forward

### Step 2: Analyze

Process the raw data into insights:

#### Meeting Load
- Total meetings this week: [count]
- Total hours in meetings: [hours]
- Trend vs last week: [up/down/same]
- Meetings that could have been emails: [flag any < 15 min or no action items]

#### Task Velocity
- Tasks completed: [count]
- Tasks added: [count]
- Net change: [completed - added] (positive = making progress)
- Overdue tasks: [count] — need attention or renegotiation

#### Communication Volume
- Emails received: [count]
- Emails sent: [count]
- Threads still open: [count]
- Response rate: [percentage of received that got replies]

#### Key Decisions Made
- Pull from Obsidian decision logs and meeting notes
- List decisions with context and owner

#### Wins This Week
- Tasks completed that moved the needle
- Projects that hit milestones
- Problems resolved

#### Carrying Forward
- Overdue tasks that need renegotiation
- Open email threads requiring response
- Decisions pending
- Items to delegate Monday morning

### Step 3: Generate the Review

Save to Obsidian as a weekly review note:

\`\`\`bash
cat > "$OBSIDIAN_VAULT/Weekly Reviews/Week of YYYY-MM-DD.md" << 'WEEKLY_EOF'
---
title: "Week of [Monday Date]"
date: [today]
tags: [weekly-review]
type: weekly-review
week-number: [WW]
---

# Week of [Monday Date] — Review

## Scorecard
| Metric | This Week | Last Week | Trend |
|--------|-----------|-----------|-------|
| Meetings | [X] | [Y] | [up/down/same] |
| Meeting Hours | [X]h | [Y]h | [up/down/same] |
| Tasks Completed | [X] | [Y] | [up/down/same] |
| Tasks Added | [X] | [Y] | [up/down/same] |
| Overdue Tasks | [X] | [Y] | [up/down/same] |
| Emails Received | [X] | [Y] | [up/down/same] |
| Emails Sent | [X] | [Y] | [up/down/same] |

## Wins
- [Win 1]
- [Win 2]
- [Win 3]

## Key Decisions
- [Decision 1] — [Context]
- [Decision 2] — [Context]

## Project Status
### [Project Name] — green/yellow/red
[One-line status summary]

### [Project Name] — green/yellow/red
[One-line status summary]

## Carrying Forward to Next Week
### Must Do Monday
- [ ] [Urgent item 1]
- [ ] [Urgent item 2]

### This Week's Priorities
- [ ] [Priority 1]
- [ ] [Priority 2]
- [ ] [Priority 3]

### Delegate
- [ ] [Task] to [Person]
- [ ] [Task] to [Person]

### Waiting On
- [Item] — from [Person] — expected [Date]

## Reflection
[Space for personal notes — what went well, what to improve, energy level]

WEEKLY_EOF
\`\`\`

### Step 4: Present Summary

After saving, present a concise verbal summary to Josh:

1. **One-line verdict**: "Strong week — shipped more than you added" or "Heavy meeting week — 22 hours in calls"
2. **Top 3 wins**
3. **Top 3 carry-forwards**
4. **One thing to delegate Monday**

Keep it tight. The full detail is in the Obsidian note.

## Tips

- Run this every Friday at 3-4pm — builds the muscle
- Compare week-over-week trends after 3+ weeks of data
- The "delegate" section is the highest-leverage part — spend time there
- If meeting hours > 20, flag it — that's too much for a CEO who needs thinking time
- The reflection section is optional but powerful over time

## Error Handling

- If Calendar MCP is unavailable → fall back to \`gws calendar\` CLI
- If Gmail MCP is unavailable → fall back to \`gws gmail\` CLI
- If Asana returns no tasks → confirm workspace connection, check \`get_me\`
- If Obsidian vault path isn't set → ask the user and set \`$OBSIDIAN_VAULT\`
- If this is the first review (no "last week" data) → skip trend columns, note it's the baseline

## See Also

- [asana](../asana/SKILL.md) — Task and project data source
- [gws-calendar-agenda](../gws-calendar-agenda/SKILL.md) — Calendar data
- [gws-gmail-triage](../gws-gmail-triage/SKILL.md) — Email triage data
- [obsidian-markdown](../obsidian-markdown/SKILL.md) — Saving the review note
- [daily-meeting-update](../daily-meeting-update/SKILL.md) — Daily version of this workflow
