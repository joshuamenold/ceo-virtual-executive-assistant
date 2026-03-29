---
name: gws-gmail-send
description: "Gmail: Send, reply to, and forward emails. Use this skill whenever the user wants to compose an email, draft a reply, forward a message, or send any kind of email communication."
---

# Gmail — Send Email

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.

## Usage
```bash
gws gmail +send --to <email> --subject <text> --body <text>
```

## Flags

| Flag | Required | Default | Description |
|------|----------|---------|-------------|
| `--to` | Yes | — | Recipient email(s), comma-separated |
| `--subject` | Yes | — | Email subject line |
| `--body` | Yes | — | Email body (plain text, or HTML with --html) |
| `--from` | — | — | Send-as alias address |
| `--cc` | — | — | CC recipient(s), comma-separated |
| `--bcc` | — | — | BCC recipient(s), comma-separated |
| `--attach` | — | — | File attachment (can be used multiple times) |
| `--html` | — | — | Treat --body as HTML content |
| `--draft` | — | — | Save as draft instead of sending |
| `--dry-run` | — | — | Show what would be sent without actually sending |

## Examples
```bash
# Simple email
gws gmail +send --to alice@example.com --subject 'Hello' --body 'Hi Alice!'

# With CC
gws gmail +send --to alice@example.com --subject 'Update' --body 'See below' --cc bob@example.com

# HTML email
gws gmail +send --to alice@example.com --subject 'Report' --body '<h1>Q2 Results</h1><p>Revenue up 15%</p>' --html

# With attachment
gws gmail +send --to alice@example.com --subject 'Report' --body 'See attached' --attach report.pdf

# Save as draft
gws gmail +send --to alice@example.com --subject 'Draft' --body 'Working on this...' --draft
```

## Tips

- Handles RFC 5322 formatting, MIME encoding, and base64 automatically.
- Use `--attach` multiple times for multiple files. Total limit: 25MB.
- Use `--draft` to save without sending — great for the user to review first.
- With `--html`, use fragment tags (`<p>`, `<b>`, `<a>`) — no `<html>` wrapper needed.

> **WRITE COMMAND:** Always confirm recipients, subject, and body with the user before executing. Use `--dry-run` first if unsure.

## See Also

- [gws-shared](../gws-shared/SKILL.md) — Global flags and auth
- [gws-gmail](../gws-gmail/SKILL.md) — Read and search email
