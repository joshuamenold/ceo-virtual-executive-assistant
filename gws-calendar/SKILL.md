---
name: gws-calendar
description: "Google Calendar: View and manage calendar events. Use this skill when the user asks about their schedule, meetings, events, or calendar in any context."
---

# Google Calendar — View and Manage

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.
```bash
gws calendar <resource> <method> [flags]
```

## Helper Commands

| Command | Description |
|---------|-------------|
| `+agenda` | Show upcoming events |
| `+insert` | Create a new event |

## Common Operations

### List events for a date range
```bash
gws calendar events list --params '{"calendarId": "primary", "timeMin": "2026-03-28T00:00:00Z", "timeMax": "2026-03-28T23:59:59Z", "singleEvents": true, "orderBy": "startTime"}'
```

### Get event details
```bash
gws calendar events get --params '{"calendarId": "primary", "eventId": "EVENT_ID"}'
```

### List all calendars
```bash
gws calendar calendarList list
```

### Check free/busy
```bash
gws calendar freebusy query --json '{"timeMin": "2026-03-28T00:00:00Z", "timeMax": "2026-03-28T23:59:59Z", "items": [{"id": "primary"}]}'
```

## Tips

- Use `singleEvents: true` to expand recurring events into individual instances.
- Use `orderBy: "startTime"` (only works with `singleEvents: true`).
- Times must be in RFC3339 format (e.g. `2026-03-28T09:00:00-04:00`).
- All read operations are safe to run without confirmation.

## See Also

- [gws-shared](../gws-shared/SKILL.md) — Global flags and auth
- [gws-calendar-agenda](../gws-calendar-agenda/SKILL.md) — Quick schedule view
- [gws-calendar-insert](../gws-calendar-insert/SKILL.md) — Create events
