---
name: slack
description: "Send and read Slack messages. Use this skill when the user wants to send a Slack message, check a channel, post an update, or communicate with their team on Slack."
---

# Slack — Send and Read Messages

## Prerequisites

Set the `SLACK_BOT_TOKEN` environment variable:
```bash
export SLACK_BOT_TOKEN="xoxb-your-bot-token"
```

To get a token:
1. Go to [api.slack.com/apps](https://api.slack.com/apps)
2. Create a new app (or use an existing one)
3. Add OAuth scopes: `chat:write`, `channels:read`, `channels:history`, `users:read`, `groups:read`, `groups:history`
4. Install to workspace and copy the Bot User OAuth Token

If the token isn't set, tell the user to follow these steps.

## Sending Messages

### Post to a channel
```bash
curl -s -X POST https://slack.com/api/chat.postMessage \
  -H "Authorization: Bearer $SLACK_BOT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "channel": "#general",
    "text": "Hello team! The deploy is complete."
  }'
```

### Send a direct message
```bash
# First, find the user's ID
curl -s https://slack.com/api/users.list \
  -H "Authorization: Bearer $SLACK_BOT_TOKEN" | jq '.members[] | select(.real_name | test("Lisa"; "i")) | {id, real_name}'

# Then send to their user ID
curl -s -X POST https://slack.com/api/chat.postMessage \
  -H "Authorization: Bearer $SLACK_BOT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "channel": "U01ABCDEF",
    "text": "Hey Lisa, can we move our 1:1 to 3pm?"
  }'
```

### Post a formatted message with blocks
```bash
curl -s -X POST https://slack.com/api/chat.postMessage \
  -H "Authorization: Bearer $SLACK_BOT_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "channel": "#engineering",
    "text": "Deploy Update",
    "blocks": [
      {"type": "header", "text": {"type": "plain_text", "text": "Deploy Complete"}},
      {"type": "section", "text": {"type": "mrkdwn", "text": "*Service:* Platform API\n*Version:* v2.4.1\n*Status:* Healthy"}}
    ]
  }'
```

## Reading Messages

### Get recent messages from a channel
```bash
# First, find the channel ID
curl -s https://slack.com/api/conversations.list \
  -H "Authorization: Bearer $SLACK_BOT_TOKEN" | jq '.channels[] | select(.name == "general") | .id'

# Then get message history
curl -s "https://slack.com/api/conversations.history?channel=C01ABCDEF&limit=10" \
  -H "Authorization: Bearer $SLACK_BOT_TOKEN" | jq '.messages[] | {user, text, ts}'
```

### List channels
```bash
curl -s https://slack.com/api/conversations.list \
  -H "Authorization: Bearer $SLACK_BOT_TOKEN" \
  -d "types=public_channel,private_channel" | jq '.channels[] | {name, id, num_members}'
```

## Slack Message Formatting (mrkdwn)

| Syntax | Result |
|--------|--------|
| `*bold*` | **bold** |
| `_italic_` | _italic_ |
| `~strikethrough~` | ~~strikethrough~~ |
| `> quote` | blockquote |
| `<@U01ABCDEF>` | @mention a user |
| `<#C01ABCDEF>` | #link a channel |

## Tips

- Reading channels is safe and doesn't require confirmation
- **Always confirm with the user** before sending any message
- Use `jq` to parse JSON responses for cleaner output
- Channel names use the `#channel` format but the API needs the channel ID — always look up the ID first
- For DMs, look up the user ID first via `users.list`
