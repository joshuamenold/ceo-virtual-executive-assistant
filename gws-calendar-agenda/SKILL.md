---
name: gws-calendar-agenda
description: "Google Calendar: Show today's or this week's schedule. Use this skill whenever the user asks 'what's on my calendar', 'what meetings do I have', 'what's my schedule', or anything about their agenda or upcoming events."
---

# Calendar — Agenda View

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.

## Usage
```bash
gws calendar +agenda
```

## Flags

| Flag | Required | Default | Description |
|------|----------|---------|-------------|
| `--today` | — | — | Show today's events |
| `--tomorrow` | — | — | Show tomorrow's events |
| `--week` | — | — | Show this week's events |
| `--days` | — | — | Number of days ahead to show |
| `--calendar` | — | all | Filter to specific calendar name or ID |
| `--timezone` | — | account default | IANA timezone (e.g. `America/New_York`) |

## Examples
```bash
# Today's agenda
gws calendar +agenda --today

# This week
gws calendar +agenda --week

# Next 3 days, table format
gws calendar +agenda --days 3 --format table

# Specific calendar
gws calendar +agenda --today --calendar 'Work'
```

## How to Present the Agenda

Format the output as a clean, scannable schedule:
```
Today — Friday, March 28, 2026

 9:00 AM  Standup (15 min) — #engineering, Google Meet
10:00 AM  1:1 with Lisa Park (30 min) — Conference Room B
11:30 AM  Board Prep Review (1 hr) — with Sarah, Michael, David
 1:00 PM  Lunch — blocked
 2:00 PM  Product Review (1 hr) — Google Meet
 3:30 PM  Focus Time (90 min) — blocked

You have 2 gaps: 10:30-11:30 AM and 3:00-3:30 PM
```

## Tips

- Read-only — never modifies events.
- Queries all calendars by default.
- Uses Google account timezone by default.

## See Also

- [gws-shared](../gws-shared/SKILL.md) — Global flags and auth
- [gws-calendar](../gws-calendar/SKILL.md) — Full calendar management
- [gws-calendar-insert](../gws-calendar-insert/SKILL.md) — Create events
