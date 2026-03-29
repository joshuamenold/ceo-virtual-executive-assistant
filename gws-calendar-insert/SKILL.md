---
name: gws-calendar-insert
description: "Google Calendar: Create a new event. Use this skill when the user wants to schedule a meeting, create a calendar event, book time, set up a call, or block time on their calendar."
---

# Calendar — Create Event

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.

## Usage
```bash
gws calendar +insert --summary <title> --start <datetime> --end <datetime>
```

## Flags

| Flag | Required | Default | Description |
|------|----------|---------|-------------|
| `--summary` | Yes | — | Event title |
| `--start` | Yes | — | Start time (ISO 8601, e.g. `2026-03-28T10:00:00-04:00`) |
| `--end` | Yes | — | End time (ISO 8601) |
| `--calendar` | — | primary | Calendar ID |
| `--location` | — | — | Event location |
| `--description` | — | — | Event description/notes |
| `--attendee` | — | — | Attendee email (can be used multiple times) |
| `--meet` | — | — | Add a Google Meet link |

## Examples
```bash
# Simple 30-min meeting
gws calendar +insert --summary 'Standup' --start '2026-03-28T09:00:00-04:00' --end '2026-03-28T09:30:00-04:00'

# With attendees and Meet link
gws calendar +insert --summary '1:1 with Lisa' --start '2026-03-28T10:00:00-04:00' --end '2026-03-28T10:30:00-04:00' --attendee lisa@company.com --meet

# With location and description
gws calendar +insert --summary 'Board Meeting' --start '2026-03-28T14:00:00-04:00' --end '2026-03-28T16:00:00-04:00' --location 'Conference Room A' --description 'Q2 review and budget approval'
```

## Tips

- Use RFC3339 format for times — always include timezone offset.
- The `--meet` flag automatically generates a Google Meet link.
- Use `--attendee` multiple times for multiple people.

> **WRITE COMMAND:** Always confirm the event title, time, and attendees with the user before executing.

## See Also

- [gws-shared](../gws-shared/SKILL.md) — Global flags and auth
- [gws-calendar-agenda](../gws-calendar-agenda/SKILL.md) — Check schedule first
- [gws-calendar](../gws-calendar/SKILL.md) — Full calendar management
