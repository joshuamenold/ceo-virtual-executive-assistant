---
name: gws-gmail-triage
description: "Gmail: Triage and prioritize unread messages. Use this skill when the user wants an inbox summary, wants to know what's urgent, asks about unread emails, or says things like 'what's in my inbox' or 'anything important in email'."
---

# Gmail — Inbox Triage

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.

## Usage
```bash
gws gmail +triage
```

## Flags

| Flag | Required | Default | Description |
|------|----------|---------|-------------|
| `--max` | — | 20 | Maximum messages to show |
| `--query` | — | `is:unread` | Gmail search query |
| `--labels` | — | — | Include label names in output |

## Examples
```bash
# Default: show unread inbox
gws gmail +triage

# Just the 5 most recent unread
gws gmail +triage --max 5

# Unread from a specific person
gws gmail +triage --query 'from:boss is:unread'

# Get JSON for further processing
gws gmail +triage --format json
```

## How to Triage for a CEO

After running triage, organize the results for the user:

1. **URGENT / Needs reply today** — Direct messages from board, investors, direct reports about time-sensitive matters
2. **IMPORTANT / This week** — Project updates, decisions needed, meeting follow-ups
3. **FYI / Can wait** — Newsletters, notifications, CC'd threads
4. **Skip** — Automated notifications, marketing, things that can be archived

Present this as a clean summary, not a raw data dump. Example output:
```
Inbox Triage — 12 unread messages

URGENT (2)
- Sarah Chen — "Q2 Board Deck — need your sign-off by EOD"
- Michael Torres — "Emergency: Production incident on Platform"

IMPORTANT (4)
- Lisa Park — "Marketing budget proposal for review"
- David Kim — "1:1 agenda for tomorrow"
...

FYI (6)
- GitHub — 3 notification emails
- Newsletter — "Weekly industry roundup"
...
```

## Tips

- Read-only — never modifies the mailbox.
- Combine with `gws-gmail` to read full message bodies of urgent items.
- Use `--format table` for a clean tabular view.

## See Also

- [gws-shared](../gws-shared/SKILL.md) — Global flags and auth
- [gws-gmail](../gws-gmail/SKILL.md) — Read and search email
- [gws-gmail-send](../gws-gmail-send/SKILL.md) — Reply to messages
