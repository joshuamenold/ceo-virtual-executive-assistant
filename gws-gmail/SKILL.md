---
name: gws-gmail
description: "Gmail: Read and search email. Use this skill whenever the user wants to check their inbox, read an email, search for messages, or look up email threads. Also use when the user mentions email, inbox, or messages in any context."
---

# Gmail — Read and Search

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.
```bash
gws gmail <resource> <method> [flags]
```

## Helper Commands

| Command | Description |
|---------|-------------|
| `+send` | Send an email |
| `+triage` | Show unread inbox summary |
| `+read` | Read a message body |
| `+reply` | Reply to a message |

## Common Operations

### List recent messages
```bash
gws gmail users messages list --params '{"maxResults": 10, "labelIds": ["INBOX"]}'
```

### Search for specific emails
```bash
gws gmail users messages list --params '{"q": "from:sarah subject:Q2 planning"}'
```

### Read a specific message
```bash
gws gmail users messages get --params '{"id": "MESSAGE_ID", "format": "full"}'
```

### List labels
```bash
gws gmail users labels list
```

## Gmail Search Query Syntax

The `q` parameter supports Gmail's search operators:

| Query | What it finds |
|-------|-------------|
| `from:alice@example.com` | Emails from Alice |
| `to:me` | Emails sent directly to you |
| `subject:quarterly review` | Emails with "quarterly review" in the subject |
| `is:unread` | Unread messages |
| `has:attachment` | Messages with attachments |
| `newer_than:2d` | Messages from the last 2 days |
| `after:2026/03/01` | Messages after March 1, 2026 |
| `label:important` | Messages labeled "important" |
| `in:inbox` | Messages in inbox |

Combine operators: `from:boss is:unread newer_than:1d`

## Tips

- All read operations are safe — they never modify the mailbox.
- Use `--format table` for human-readable output.
- Use `--format json` when you need to parse the output.
- For large result sets, use `--page-all` with `--page-limit`.

## See Also

- [gws-shared](../gws-shared/SKILL.md) — Global flags and auth
- [gws-gmail-send](../gws-gmail-send/SKILL.md) — Send email
- [gws-gmail-triage](../gws-gmail-triage/SKILL.md) — Inbox triage
