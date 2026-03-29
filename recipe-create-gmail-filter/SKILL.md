---
name: recipe-create-gmail-filter
description: "Create Gmail filters for automatic email organization. Use this skill when the user wants to auto-sort emails, set up inbox rules, filter newsletters, auto-label messages, or organize their email flow."
---

# Recipe: Create Gmail Filter

Set up Gmail filters to automatically organize incoming email.

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.

## How Filters Work

Gmail filters match incoming messages against criteria (from, to, subject, keywords) and apply actions (label, archive, star, forward, etc.).

## Creating a Filter
```bash
gws gmail users settings filters create --json '{
  "criteria": {
    "from": "notifications@github.com"
  },
  "action": {
    "addLabelIds": ["LABEL_ID"],
    "removeLabelIds": ["INBOX"]
  }
}'
```

## Common Filter Recipes

### Auto-label newsletters and remove from inbox
```bash
# First, create the label
gws gmail users labels create --json '{"name": "Newsletters", "labelListVisibility": "labelShow"}'

# Note the label ID from the response, then create the filter
gws gmail users settings filters create --json '{
  "criteria": {
    "from": "newsletter@company.com"
  },
  "action": {
    "addLabelIds": ["Label_123"],
    "removeLabelIds": ["INBOX"]
  }
}'
```

### Star emails from VIPs
```bash
gws gmail users settings filters create --json '{
  "criteria": {
    "from": "ceo@partner.com investor@vc.com board@company.com"
  },
  "action": {
    "addLabelIds": ["STARRED"]
  }
}'
```

### Auto-archive notifications
```bash
gws gmail users settings filters create --json '{
  "criteria": {
    "from": "noreply@",
    "subject": "notification"
  },
  "action": {
    "removeLabelIds": ["INBOX"]
  }
}'
```

## Filter Criteria Options

| Field | Description | Example |
|-------|-------------|---------|
| `from` | Sender address(es) | `"boss@company.com"` |
| `to` | Recipient address | `"team-list@company.com"` |
| `subject` | Subject contains | `"weekly report"` |
| `query` | Full Gmail search query | `"has:attachment larger:5M"` |
| `hasAttachment` | Has attachments | `true` |

## Filter Action Options

| Field | Description |
|-------|-------------|
| `addLabelIds` | Apply label(s) |
| `removeLabelIds` | Remove label(s) — use `["INBOX"]` to archive |
| `forward` | Forward to another address |

## Listing Existing Filters
```bash
gws gmail users settings filters list
```

## Tips

- Always show the user the filter criteria and actions before creating
- Create the label first if it doesn't exist
- Use `from` with space-separated addresses to match multiple senders
- Use `query` for complex matching (same syntax as Gmail search)
- Test the criteria with a Gmail search first before creating the filter

> **WRITE COMMAND:** Always confirm filter rules with the user before creating.

## Skills Used

- [gws-gmail](../gws-gmail/SKILL.md)
- [gws-shared](../gws-shared/SKILL.md)
