---
name: obsidian-shared
description: "Shared configuration and conventions for all Obsidian skills. Read this skill before using any obsidian-* skill."
---

# Obsidian — Shared Reference

All Obsidian skills work by reading and writing markdown files directly in the user's vault.

## Configuration

**Set your vault path below** (edit this line):
```
OBSIDIAN_VAULT=~/Documents/MyVault
```

If this path doesn't exist, tell the user to edit this file and set it to their actual Obsidian vault location.

## Vault Conventions

- All notes are `.md` files (standard markdown with Obsidian extensions)
- Every note should have YAML frontmatter at the top
- Use `[[wikilinks]]` for internal links between notes
- Use `#tags` for categorization
- Respect the user's existing folder structure — don't create new top-level folders without asking

## Standard Frontmatter

Every note you create should include at minimum:
```yaml
---
title: Note Title
date: 2026-03-28
tags: [meeting, q2]
---
```

Add additional fields as relevant: `status`, `project`, `people`, `priority`, etc.

## Obsidian-Specific Markdown

| Syntax | What it does |
|--------|-------------|
| `[[Note Name]]` | Link to another note |
| `[[Note Name\|Display Text]]` | Link with custom display text |
| `![[Note Name]]` | Embed another note inline |
| `#tag` | Tag |
| `#tag/subtag` | Nested tag |
| `> [!note]` | Callout block (note, warning, tip, important) |
| `> [!todo]` | Todo callout |
| `- [ ]` | Checkbox / task |
| `- [x]` | Completed task |

## File Operations

Obsidian vaults are just folders of markdown files. Use standard file tools:
```bash
# List all notes in the vault
find "$OBSIDIAN_VAULT" -name "*.md" -type f

# Search note contents
grep -rl "search term" "$OBSIDIAN_VAULT" --include="*.md"

# Search note titles
find "$OBSIDIAN_VAULT" -name "*keyword*" -type f

# List folders in vault root
ls -d "$OBSIDIAN_VAULT"/*/

# Count notes
find "$OBSIDIAN_VAULT" -name "*.md" -type f | wc -l
```

## Security Rules

- **Never delete notes** without explicit user confirmation
- **Never overwrite** an existing note without showing the user what will change
- Creating new notes and reading notes are always safe
- When editing, read the file first, make changes, and show the user before writing
