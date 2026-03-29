---
name: obsidian-markdown
description: "Create and edit Obsidian notes with proper markdown and frontmatter. Use this skill whenever the user wants to create a note, write meeting notes, save a summary, jot something down, or edit an existing note in their vault."
---

# Obsidian — Create and Edit Notes

> **PREREQUISITE:** Read `../obsidian-shared/SKILL.md` for vault path, conventions, and security rules.

## Creating a New Note
```bash
VAULT="$OBSIDIAN_VAULT"  # From obsidian-shared config

cat > "$VAULT/Notes/My New Note.md" << 'EOF'
---
title: My New Note
date: 2026-03-28
tags: [meeting, project-alpha]
---

# My New Note

Content goes here.

## Key Points

- Point one
- Point two

## Action Items

- [ ] Follow up with Sarah
- [ ] Review the budget proposal
EOF
```

## Editing an Existing Note

Always read the note first, then modify:
```bash
# Read current content
cat "$VAULT/Notes/Existing Note.md"

# Then write the updated version (after showing user the changes)
```

## Note Templates by Type

### Meeting Notes
```markdown
---
title: "Meeting: [Topic]"
date: 2026-03-28
tags: [meeting]
attendees: [person1, person2]
project: project-name
---

# Meeting: [Topic]

**Date:** 2026-03-28
**Attendees:** [[Person 1]], [[Person 2]]

## Agenda

1. Topic one
2. Topic two

## Discussion

- Key point discussed

## Decisions

- Decision made

## Action Items

- [ ] @person — Task description — Due: date
```

### Daily Note
```markdown
---
title: "2026-03-28"
date: 2026-03-28
tags: [daily]
---

# Friday, March 28, 2026

## Schedule

- 9:00 AM — Standup
- 10:00 AM — 1:1 with Lisa

## Top Priorities

- [ ] Priority one
- [ ] Priority two

## Notes

-

## End of Day Recap

-
```

### Decision Log
```markdown
---
title: "Decision: [Topic]"
date: 2026-03-28
tags: [decision]
status: decided
---

# Decision: [Topic]

**Context:** Why this decision was needed
**Decision:** What was decided
**Alternatives considered:** What else was on the table
**Rationale:** Why this was chosen
**Owner:** Who is responsible
**Review date:** When to revisit
```

### Person Note (lightweight CRM)
```markdown
---
title: "Person Name"
date: 2026-03-28
tags: [person, contact]
company: Company Name
role: Their Title
---

# Person Name

**Company:** Company Name
**Role:** Their Title
**Email:** email@example.com

## Context

How you know them, relationship notes.

## Meeting History

- 2026-03-28 — Discussed partnership opportunity

## Notes

-
```

## Tips

- Save notes in the folder that matches the user's vault structure (check existing folders first)
- Use descriptive filenames — `Meeting - Q2 Board Review.md` not `meeting.md`
- Always link to related people and projects using `[[wikilinks]]`
- Use callouts for important highlights: `> [!important]`
- Creating and reading notes is always safe; editing requires showing the diff first

## See Also

- [obsidian-shared](../obsidian-shared/SKILL.md) — Vault config and conventions
- [obsidian-bases](../obsidian-bases/SKILL.md) — Structured data views
- [obsidian-cli](../obsidian-cli/SKILL.md) — Search and manage vault
