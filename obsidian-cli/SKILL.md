---
name: obsidian-cli
description: "Search, find, and manage files in an Obsidian vault via the command line. Use this skill when the user wants to search their notes, find a specific note, list recent notes, check what's in their vault, or do bulk operations on vault files."
---

# Obsidian — CLI Search and Management

> **PREREQUISITE:** Read `../obsidian-shared/SKILL.md` for vault path, conventions, and security rules.

## Searching Notes

### Search by content (full-text)
```bash
grep -rl "search term" "$OBSIDIAN_VAULT" --include="*.md"
```

### Search with context (show matching lines)
```bash
grep -rn "search term" "$OBSIDIAN_VAULT" --include="*.md" --color=always
```

### Search by filename
```bash
find "$OBSIDIAN_VAULT" -name "*keyword*" -type f
```

### Search by tag in frontmatter
```bash
grep -rl "tags:.*meeting" "$OBSIDIAN_VAULT" --include="*.md"
```

### Search by frontmatter property
```bash
# Find all notes with status: in-progress
grep -rl "status: in-progress" "$OBSIDIAN_VAULT" --include="*.md"

# Find all notes assigned to a person
grep -rl "owner: Sarah" "$OBSIDIAN_VAULT" --include="*.md"
```

## Listing Notes

### Recent notes (by modification time)
```bash
find "$OBSIDIAN_VAULT" -name "*.md" -type f -printf '%T@ %p\n' | sort -rn | head -20 | cut -d' ' -f2-
```

On macOS (no `-printf`):
```bash
find "$OBSIDIAN_VAULT" -name "*.md" -type f -exec stat -f '%m %N' {} \; | sort -rn | head -20 | cut -d' ' -f2-
```

### Notes in a specific folder
```bash
ls "$OBSIDIAN_VAULT/Meetings/"
```

### Count notes by folder
```bash
find "$OBSIDIAN_VAULT" -name "*.md" -type f | sed "s|$OBSIDIAN_VAULT/||" | cut -d/ -f1 | sort | uniq -c | sort -rn
```

## Reading Notes

### Read a note
```bash
cat "$OBSIDIAN_VAULT/Path/To/Note.md"
```

### Read just the frontmatter
```bash
sed -n '/^---$/,/^---$/p' "$OBSIDIAN_VAULT/Path/To/Note.md"
```

### Read just the body (skip frontmatter)
```bash
sed '1,/^---$/d; /^---$/,1d' "$OBSIDIAN_VAULT/Path/To/Note.md" | tail -n +2
```

## Finding Broken Links
```bash
grep -roh '\[\[[^]]*\]\]' "$OBSIDIAN_VAULT" --include="*.md" | sort -u
```

## Tips

- All search and list operations are read-only and safe to run
- Use macOS-compatible commands (this stack assumes macOS by default)
- When the user says "find that note about X", use content search first, then filename search
- For bulk operations (rename, move, re-tag), always show the user what will change before executing

## See Also

- [obsidian-shared](../obsidian-shared/SKILL.md) — Vault config and conventions
- [obsidian-markdown](../obsidian-markdown/SKILL.md) — Create and edit notes
- [obsidian-bases](../obsidian-bases/SKILL.md) — Structured data views
