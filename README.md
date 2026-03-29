# CEO Virtual Executive Assistant

Turn **Claude Code + Obsidian** into your virtual executive assistant.

Automate email, calendar, Slack, notes, task management, web research, and daily workflows — so you can focus on leading, not admin.

---

## What This Is

A collection of **Agent Skills** for Claude Code that give you a hands-free executive assistant. Talk to Claude in plain English and it will read your email, check your calendar, draft replies, create meeting notes, search the web, and manage your Obsidian vault — all from the command line.

**Example commands you can give Claude:**

- "What's on my calendar today?"
- "Triage my inbox and flag anything urgent"
- "Draft a reply to the email from Sarah about Q2 planning"
- "Create a note in Obsidian summarizing today's meetings"
- "Find 30 minutes free this week for a 1:1 with marketing"
- "Search the web for the latest on our competitor's product launch"
- "Create a presentation outline for the board meeting"
- "Send a Slack message to #engineering saying the deploy is done"

---

## Prerequisites

Before you start, make sure you have:

1. **Claude Code** installed and working ([install guide](https://docs.anthropic.com/en/docs/claude-code))
2. **Node.js 18+** (`node --version` to check)
3. **An Obsidian vault** on your local machine

---

## Setup (15 minutes)

### Step 1: Install the Google Workspace CLI

The `gws` CLI is what connects Claude to Gmail, Calendar, Docs, Drive, and Slides. Install it:
```bash
# macOS / Linux
curl -fsSL https://raw.githubusercontent.com/googleworkspace/cli/main/install.sh | bash
```

Verify it works:
```bash
gws --version
```

Then authenticate with your Google account:
```bash
gws auth login
```

This opens your browser for OAuth consent. Sign in with the Google account you want Claude to manage. The token is saved locally — you only need to do this once.

> **Tip:** If you use Google Workspace (company email), your admin may need to approve the OAuth scopes. See the [gws docs](https://github.com/googleworkspace/cli) for details.

### Step 2: Install the Skills

Clone this repo and install the skills into your Claude Code project:
```bash
# Navigate to your project directory (or wherever you run Claude Code)
cd ~/my-project

# Clone the skills
git clone https://github.com/joshuamenold/ceo-virtual-executive-assistant.git /tmp/ceo-ea

# Copy skills into your .claude/skills directory
mkdir -p .claude/skills
cp -r /tmp/ceo-ea/skills/* .claude/skills/

# Copy the CLAUDE.md to your project root
cp /tmp/ceo-ea/CLAUDE.md ./CLAUDE.md
```

### Step 3: Configure Your Obsidian Vault

Open the file `.claude/skills/obsidian-shared/SKILL.md` and set your vault path:
```
OBSIDIAN_VAULT=~/Documents/MyVault
```

Replace `~/Documents/MyVault` with the actual path to your Obsidian vault.

### Step 4: Set Up Slack (Optional)

If you want Slack integration, you'll need a Slack CLI token:

1. Go to [api.slack.com/apps](https://api.slack.com/apps) and create a new app
2. Under **OAuth & Permissions**, add scopes: `chat:write`, `channels:read`, `channels:history`, `users:read`
3. Install the app to your workspace and copy the **Bot User OAuth Token**
4. Set it as an environment variable:
```bash
export SLACK_BOT_TOKEN="xoxb-your-token-here"
```

Add this to your shell profile (`~/.zshrc` or `~/.bashrc`) so it persists.

### Step 5: Set Up Web Search (Optional)

For web search, get a free Tavily API key:

1. Sign up at [tavily.com](https://tavily.com)
2. Copy your API key
3. Set it as an environment variable:
```bash
export TAVILY_API_KEY="tvly-your-key-here"
```

### Step 6: Verify Everything Works

Start Claude Code and try:
```
> What's on my calendar today?
```

If you see your agenda, you're all set.

---

## Included Skills

### Google Workspace (requires `gws` CLI)

| Skill | What It Does |
|-------|-------------|
| `gws-shared` | Shared auth, flags, and security rules for all GWS skills |
| `gws-gmail` | Read and search your Gmail inbox |
| `gws-gmail-send` | Compose and send emails |
| `gws-gmail-triage` | Scan and prioritize unread messages |
| `gws-calendar` | View and manage Google Calendar events |
| `gws-calendar-agenda` | Show today's or this week's schedule |
| `gws-calendar-insert` | Create new calendar events |
| `gws-docs` | Read Google Docs content |
| `gws-docs-write` | Create and edit Google Docs |
| `gws-drive` | Browse and manage Google Drive files |

### Obsidian (requires local vault)

| Skill | What It Does |
|-------|-------------|
| `obsidian-shared` | Shared config and conventions for Obsidian skills |
| `obsidian-markdown` | Create and edit notes with proper Obsidian markdown |
| `obsidian-bases` | Use Obsidian Bases for structured data views |
| `obsidian-cli` | Search, open, and manage vault files via CLI |

### Project Management

| Skill | What It Does |
|-------|-------------|
| `asana` | Manage Asana tasks, projects, portfolios, and status updates |

### Communication

| Skill | What It Does |
|-------|-------------|
| `slack` | Send and read Slack messages |

### Research

| Skill | What It Does |
|-------|-------------|
| `tavily-search` | Search the web for current information |

### CEO Recipes (multi-skill workflows)

| Skill | What It Does |
|-------|-------------|
| `recipe-daily-briefing` | Generate a morning briefing from calendar + email + notes |
| `recipe-find-free-time` | Find open slots across calendars for scheduling |
| `recipe-create-gmail-filter` | Set up Gmail filters for automatic organization |
| `recipe-create-presentation` | Generate presentation outlines in Google Slides |
| `recipe-weekly-review` | End-of-week CEO briefing across calendar, tasks, and email |
| `writing-plans` | Structure long-form writing with outlines and drafts |

---

## Troubleshooting

**"gws: command not found"**
The `gws` CLI isn't installed or isn't on your PATH. Re-run the install script and restart your terminal.

**"Error: unauthorized" from Gmail/Calendar**
Run `gws auth login` to refresh your OAuth token.

**"OBSIDIAN_VAULT not set"**
Edit `.claude/skills/obsidian-shared/SKILL.md` and set the path to your vault.

**"SLACK_BOT_TOKEN not set"**
Export the token in your terminal: `export SLACK_BOT_TOKEN="xoxb-..."`.

**Skills not loading in Claude Code**
Make sure your skills are in `.claude/skills/` relative to the directory where you run Claude Code. Each skill should be a folder containing a `SKILL.md` file.

---

## License

MIT
