---
name: recipe-find-free-time
description: "Find open time slots in your calendar for scheduling. Use this skill when the user wants to find free time, schedule a meeting, find availability, book a slot, or asks 'when am I free' or 'find time for a meeting with X'."
---

# Recipe: Find Free Time

This is a **multi-skill workflow** that analyzes your calendar to find available slots.

## How It Works

### Step 1: Get events for the date range
```bash
gws calendar +agenda --week --format json
```

Or for a specific range:
```bash
gws calendar events list --params '{
  "calendarId": "primary",
  "timeMin": "2026-03-28T00:00:00Z",
  "timeMax": "2026-04-04T23:59:59Z",
  "singleEvents": true,
  "orderBy": "startTime"
}' --format json
```

### Step 2: Analyze gaps

Parse the events and find gaps between them. Consider:

- **Working hours only** — Default to 9:00 AM-5:00 PM unless the user specifies otherwise
- **Minimum slot duration** — If looking for a meeting slot, filter out gaps shorter than the meeting length
- **Buffer time** — Leave 15 minutes between back-to-back meetings when possible
- **Lunch block** — Respect 12:00-1:00 PM as lunch unless the user says otherwise

### Step 3: Present available slots
```
Available slots this week (30 min or longer):

Monday, March 30:
  10:30 AM - 11:30 AM (1 hr)
   3:00 PM -  5:00 PM (2 hrs)

Tuesday, March 31:
   9:00 AM - 10:00 AM (1 hr)
   2:00 PM -  3:30 PM (1.5 hrs)

Best slot for a 1-hour meeting: Monday 3:00 PM or Tuesday 2:00 PM
```

### Step 4: Book it (if requested)
```bash
gws calendar +insert --summary 'Meeting Title' --start '2026-03-30T15:00:00-04:00' --end '2026-03-30T16:00:00-04:00' --attendee person@example.com --meet
```

## Tips

- Always ask the user: how long is the meeting, and what days/times work?
- Default to the user's timezone from their Google account
- If checking multiple people's availability, use the `freebusy` API:
```bash
  gws calendar freebusy query --json '{
    "timeMin": "2026-03-28T00:00:00Z",
    "timeMax": "2026-04-04T23:59:59Z",
    "items": [{"id": "primary"}, {"id": "other@example.com"}]
  }'
```
- Suggest the "best" slot — usually the earliest one that doesn't create a back-to-back situation

## Skills Used

- [gws-calendar-agenda](../gws-calendar-agenda/SKILL.md)
- [gws-calendar](../gws-calendar/SKILL.md)
- [gws-calendar-insert](../gws-calendar-insert/SKILL.md)
