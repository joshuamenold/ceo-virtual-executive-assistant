---
name: delegation-tracker
description: "Track delegated tasks, follow up on overdue items, and surface what's waiting on others. Use this skill when the user says 'what did I delegate', 'who owes me what', 'follow up', 'delegation tracker', 'what\'s pending from my team', 'who\'s behind', or wants to log a new delegation. Also trigger when the user finishes a meeting and says 'I need to delegate X to Y' or 'remind me to follow up on Z'."
---

# Delegation Tracker — CEO Follow-Up System

Track what you've delegated, to whom, when it's due, and surface items needing follow-up.

## Why This Matters

As a CEO running four divisions, you delegate constantly. The failure mode isn't bad delegation — it's losing track of what you delegated. This skill makes sure nothing falls through the cracks.

## Data Sources

This skill pulls from two sources and writes to one:

### Read From
1. **Asana** — tasks assigned to others by you, with due dates
   - \`search_tasks_preview\` with assignee filters
   - \`get_my_tasks\` → filter for tasks you created but assigned to others
   - \`get_tasks\` per project → filter by assignee

2. **Obsidian** — delegation log entries from meeting notes and daily notes
   \`\`\`bash
   grep -r "delegated\\|assigned to\\|→.*by\\|follow up with" "$OBSIDIAN_VAULT" --include="*.md" -l
   \`\`\`

### Write To
- **Obsidian** — \`$OBSIDIAN_VAULT/Delegation/Delegation Log.md\`

## Workflows

### "What did I delegate?" / "Who owes me what?"

1. Pull Asana tasks where:
   - You are the creator or collaborator
   - Assignee is NOT you
   - Task is not completed
2. Pull Obsidian delegation log entries marked as open
3. Merge and deduplicate
4. Present grouped by person:

\`\`\`
## Delegations by Person

### Sarah Chen (VP Operations)
🔴 OVERDUE
   • Q2 budget reconciliation — Due Mar 20 (8 days overdue)
   • Vendor contract review — Due Mar 24 (4 days overdue)

🟡 DUE THIS WEEK
   • Hire posting for project manager — Due Mar 31

🟢 ON TRACK
   • Safety training rollout — Due Apr 15
   • Fleet maintenance schedule — Due Apr 30

### Mike Torres (Cornerstone GM)
🟡 DUE THIS WEEK
   • March revenue report — Due Mar 28

🟢 ON TRACK
   • Leap CRM migration plan — Due Apr 10
\`\`\`

### "Log a delegation"

When Josh says "I need X to do Y by Z":
1. Create an Asana task assigned to the person
2. Log it in the Obsidian delegation file
3. Confirm both were created

\`\`\`bash
cat >> "$OBSIDIAN_VAULT/Delegation/Delegation Log.md" << 'EOF'

### [Date] — [Task Name]
- **Delegated to**: [Person]
- **Due**: [Date]
- **Context**: [Why / meeting it came from]
- **Asana**: [link if created]
- **Status**: open
EOF
\`\`\`

### "Follow up on overdue items"

1. Pull all overdue delegations
2. For each, draft a follow-up message:
   - Use the \`che-email-composer\` skill for email drafts
   - Or draft Slack messages
3. Present drafts for approval before sending
4. After sending, update the delegation log with follow-up date

### "Weekly delegation review"

Run as part of the \`recipe-weekly-review\` or standalone:
1. Pull all active delegations
2. Calculate stats:
   - Total active delegations: [count]
   - Overdue: [count]
   - Completed this week: [count]
   - New this week: [count]
3. Flag people with 3+ overdue items
4. Suggest re-delegation or deadline renegotiation where needed

## Delegation Log Format (Obsidian)

\`\`\`markdown
---
title: "Delegation Log"
date: 2026-03-28
tags: [delegation, tracking, ceo]
type: delegation-log
---

# Active Delegations

## By Person

### [Person Name]
| Task | Due | Status | Asana Link | Notes |
|------|-----|--------|------------|-------|
| [Task] | [Date] | open/overdue/done | [link] | [context] |

## Recently Completed
| Task | Delegated To | Completed | Days to Complete |
|------|-------------|-----------|-----------------|
| [Task] | [Person] | [Date] | [N days] |
\`\`\`

## CEO Nudge Templates

When following up, tone matters. These are direct but respectful:

**Gentle check-in** (1-2 days overdue):
> "Hey [Name], just checking in on [task]. Where are we on this? Need anything from me?"

**Direct follow-up** (3-7 days overdue):
> "[Name], [task] was due [date]. What's the blocker? Let's get this resolved this week."

**Escalation** (7+ days overdue):
> "[Name], [task] is now [N] days overdue. I need a status update today and a revised completion date. If you're blocked, let's talk."

## Integration with Other Skills

- **After meetings**: When \`meeting-transcript-processor\` extracts action items assigned to others, auto-log them here
- **Weekly review**: \`recipe-weekly-review\` pulls delegation stats into the scorecard
- **Email drafts**: \`che-email-composer\` drafts follow-up messages for overdue items

## Error Handling

- If Asana search returns too many results → narrow by project or date range
- If a person isn't in Asana → log in Obsidian only, note "not in Asana"
- If delegation log doesn't exist → create it with the template above
- Always confirm before sending follow-up messages

## See Also

- [asana](../asana/SKILL.md) — Task management backend
- [che-email-composer](../che-email-composer/SKILL.md) — Draft follow-up messages
- [recipe-weekly-review](../recipe-weekly-review/SKILL.md) — Weekly delegation stats
- [obsidian-markdown](../obsidian-markdown/SKILL.md) — Delegation log storage
