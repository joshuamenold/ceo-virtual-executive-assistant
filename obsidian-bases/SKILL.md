---
name: obsidian-bases
description: "Use Obsidian Bases for database-like structured views of your notes. Use this skill when the user wants to track projects, manage tasks across notes, create dashboards, or build structured views of their vault data like tables, kanban boards, or filtered lists."
---

# Obsidian — Bases (Structured Data Views)

> **PREREQUISITE:** Read `../obsidian-shared/SKILL.md` for vault path, conventions, and security rules.

## What Are Bases?

Bases let you create database-like views of your notes using frontmatter properties as columns. Think of it as a spreadsheet that queries your vault — you can filter, sort, and group notes by any property.

## How It Works

Bases work by querying notes that share common frontmatter properties. The key is consistent frontmatter across related notes.

## Setting Up Structured Notes

### Step 1: Create notes with consistent frontmatter

For project tracking, every project note should have the same fields:
```markdown
---
title: Project Alpha
type: project
status: in-progress
priority: high
owner: Sarah Chen
due: 2026-04-15
tags: [project, engineering]
---

# Project Alpha

Project details here...
```

### Step 2: Create a Base file

Create a `.base` file (or use Obsidian's Bases UI) that queries these notes:
```
# Project Tracker

Filter: type = "project"
Columns: title, status, priority, owner, due
Sort: due ascending
Group by: status
```

## Common Use Cases for a CEO

### Project Portfolio Tracker
```yaml
---
type: project
status: in-progress  # not-started, in-progress, blocked, complete
priority: high       # low, medium, high, critical
owner: Person Name
due: 2026-04-15
budget: 50000
---
```

### Meeting Action Items
```yaml
---
type: action-item
status: open         # open, in-progress, done
assignee: Person Name
source: "[[Meeting - Board Review]]"
due: 2026-04-01
---
```

### People / Lightweight CRM
```yaml
---
type: person
company: Acme Corp
role: VP Engineering
relationship: partner    # team, investor, partner, vendor, advisor
last-contact: 2026-03-20
---
```

### OKR Tracking
```yaml
---
type: okr
quarter: Q2-2026
objective: "Grow revenue 20%"
progress: 45           # percentage
owner: Executive Team
status: on-track       # on-track, at-risk, behind
---
```

## Tips

- The power of Bases comes from **consistent frontmatter** — make sure all related notes use the same property names and value formats
- Use `type` as the primary way to categorize notes (project, person, action-item, decision, etc.)
- Use dropdowns/enums for status fields so they're easy to filter
- When creating notes for the user, always include the frontmatter properties that match their existing Bases

## See Also

- [obsidian-shared](../obsidian-shared/SKILL.md) — Vault config and conventions
- [obsidian-markdown](../obsidian-markdown/SKILL.md) — Create and edit notes
- [obsidian-cli](../obsidian-cli/SKILL.md) — Search the vault
