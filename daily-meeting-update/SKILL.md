---
name: recipe-daily-briefing
description: "Generate a morning briefing combining calendar, email, and notes. Use this skill when the user asks for a daily briefing, morning update, 'what's going on today', daily summary, or standup prep. This is the CEO's morning dashboard."
---

# Recipe: Daily Briefing

This is a **multi-skill workflow** that pulls data from calendar, email, and Obsidian to create a comprehensive morning briefing.

## How It Works

Run these skills in sequence:

### Step 1: Get today's calendar
```bash
gws calendar +agenda --today --format json
```

### Step 2: Triage inbox
```bash
gws gmail +triage --max 15
```

### Step 3: Check yesterday's notes (optional)
```bash
# Find yesterday's daily note
cat "$OBSIDIAN_VAULT/Daily/$(date -v-1d +%Y-%m-%d).md" 2>/dev/null

# Find open action items
grep -rn "\- \[ \]" "$OBSIDIAN_VAULT" --include="*.md" | head -20
```

### Step 4: Compile the Briefing

Format everything into a clean morning brief:
```markdown
# Daily Briefing — Friday, March 28, 2026

## Today's Schedule (6 meetings, 2 gaps)

 9:00 AM  Standup (15 min) — #engineering
10:00 AM  1:1 with Lisa Park (30 min) — Conf Room B
11:30 AM  Board Prep Review (1 hr) — Sarah, Michael, David
 1:00 PM  Lunch — blocked
 2:00 PM  Product Review (1 hr) — Google Meet
 3:30 PM  Focus Time (90 min) — blocked

Open slots: 10:30-11:30 AM, 3:00-3:30 PM

## Email Summary (12 unread)

URGENT (2)
- Sarah Chen — "Q2 Board Deck — need sign-off by EOD"
- Michael Torres — "Production incident on Platform"

IMPORTANT (4)
- Lisa Park — "Marketing budget proposal"
- David Kim — "1:1 agenda for today"

FYI (6)
- 3 GitHub notifications, 2 newsletters, 1 calendar update

## Open Action Items (from yesterday)

- [ ] Review Q2 board deck (due today)
- [ ] Send feedback on marketing proposal
- [ ] Approve PTO request from engineering

## Today's Priorities

1. Sign off on Q2 board deck before the 11:30 prep meeting
2. Check on the production incident with Michael
3. Review marketing budget before Lisa's 1:1
```

### Step 5: Save to Obsidian (optional)
```bash
cat > "$OBSIDIAN_VAULT/Daily/$(date +%Y-%m-%d).md" << 'EOF'
---
title: "2026-03-28"
date: 2026-03-28
tags: [daily, briefing]
---

[Briefing content here]
EOF
```

## Tips

- Run this first thing in the morning or when the user says "what's going on today"
- Be proactive — if there's a meeting in 30 minutes, mention what emails or notes are relevant to it
- Highlight conflicts and time crunches
- The priority list should be actionable and ordered by urgency

## Skills Used

- [gws-calendar-agenda](../gws-calendar-agenda/SKILL.md)
- [gws-gmail-triage](../gws-gmail-triage/SKILL.md)
- [obsidian-cli](../obsidian-cli/SKILL.md)
- [obsidian-markdown](../obsidian-markdown/SKILL.md)
