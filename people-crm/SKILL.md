---
name: people-crm
description: "Manage a personal CRM in Obsidian — track relationships, log interactions, surface who you haven't talked to recently, and prep context before meetings. Use this skill when the user says 'who haven't I talked to', 'add a note about my call with X', 'prep me for my meeting with Y', 'update the CRM', 'relationship tracker', 'when did I last talk to Z', or wants to create or update a person note. Also trigger when the user mentions a person by name and wants context on past interactions."
---

# People CRM — Relationship Intelligence

Track relationships, log interactions, and surface relationship health — all in your Obsidian vault.

## Why This Matters

Your network is your leverage. As a CEO, the quality of your relationships with direct reports, board members, clients, vendors, pastors, and key contacts directly impacts your effectiveness. This skill makes sure you never walk into a meeting cold and never let an important relationship go stale.

## Vault Structure

\`\`\`
$OBSIDIAN_VAULT/
  People/
    Sarah Chen.md
    Mike Torres.md
    [Name].md
  People/Templates/
    Person Template.md
\`\`\`

## Person Note Template

\`\`\`bash
cat > "$OBSIDIAN_VAULT/People/[Full Name].md" << 'PERSON_EOF'
---
title: "[Full Name]"
date: [created date]
tags: [people, crm]
type: person
role: "[Title/Role]"
company: "[Company/Division]"
category: "[direct-report|board|client|vendor|pastor|elder|personal|other]"
email: "[email]"
phone: "[phone]"
last-contact: [YYYY-MM-DD]
contact-frequency: "[weekly|biweekly|monthly|quarterly]"
relationship-health: "[strong|good|needs-attention|at-risk]"
---

# [Full Name]

## Quick Context
- **Role**: [Title] at [Company/Division]
- **Reports to**: [if applicable]
- **Key projects**: [what you work on together]
- **Communication style**: [direct, detail-oriented, needs warming up, etc.]
- **Personal notes**: [family details, interests, birthday, etc.]

## Interaction Log

### [Date] — [Meeting/Call/Email]
- **Context**: [what it was about]
- **Key points**: [what was discussed]
- **Action items**: [what came out of it]
- **Mood/Tone**: [how did it go]

## Notes
[Running notes about this person — things to remember, context for future interactions]

PERSON_EOF
\`\`\`

## Workflows

### "Prep me for my meeting with [Name]"

1. Look up the person note in \`$OBSIDIAN_VAULT/People/[Name].md\`
2. Check today's calendar for the meeting details (time, agenda, attendees)
3. Pull recent Asana tasks involving this person
4. Search Gmail for recent email threads with them
5. Present a prep brief:

\`\`\`
## Meeting Prep: Sarah Chen
📅 Today 2:00 PM — Quarterly Operations Review (45 min)

### Relationship Context
- VP Operations, CHE Companies — reports to you
- Last 1:1: Mar 14 (14 days ago)
- Relationship: Strong
- Style: Detail-oriented, appreciates data, doesn't like surprises

### Recent Interactions
- Mar 14: 1:1 — discussed Q1 close, she flagged concerns about fleet costs
- Mar 10: Email thread — vendor contract negotiation (still open)
- Mar 7: Leadership meeting — presented ops dashboard

### Open Items Between You
- Q2 budget reconciliation (overdue, due Mar 20)
- Vendor contract review (due Mar 24)
- Safety training rollout (on track, due Apr 15)

### Personal Notes
- Daughter's soccer tournament last weekend — ask how it went
- Anniversary coming up in April
\`\`\`

### "Log an interaction with [Name]"

1. Find or create the person note
2. Append a new interaction entry with date, context, key points
3. Update \`last-contact\` frontmatter
4. Recalculate \`relationship-health\` based on contact frequency

\`\`\`bash
# Update frontmatter
sed -i 's/last-contact: .*/last-contact: 2026-03-28/' "$OBSIDIAN_VAULT/People/[Name].md"

# Append interaction
cat >> "$OBSIDIAN_VAULT/People/[Name].md" << 'LOG_EOF'

### 2026-03-28 — [Type]
- **Context**: [what it was about]
- **Key points**: [summary]
- **Action items**: [follow-ups]
- **Mood/Tone**: [assessment]
LOG_EOF
\`\`\`

### "Who haven't I talked to recently?"

1. Scan all person notes for \`last-contact\` and \`contact-frequency\`
2. Calculate who's overdue:
   - weekly contact expected → flag if > 10 days
   - biweekly → flag if > 20 days
   - monthly → flag if > 40 days
   - quarterly → flag if > 100 days
3. Present grouped by urgency:

\`\`\`
## Relationships Needing Attention

🔴 OVERDUE (should have contacted by now)
   • Pastor Williams — Last contact: Feb 12 (44 days) — monthly cadence
   • Tom Bradley (board) — Last contact: Jan 30 (57 days) — monthly cadence

🟡 COMING DUE (contact soon)
   • Dave Kim (vendor) — Last contact: Mar 5 (23 days) — monthly cadence
   • Lisa Park (client) — Last contact: Mar 10 (18 days) — biweekly cadence

🟢 ON TRACK
   • Sarah Chen — Last contact: Mar 14 (14 days) — biweekly cadence
   • Mike Torres — Last contact: Mar 26 (2 days) — weekly cadence
\`\`\`

### "Create a person note for [Name]"

1. Ask for: name, role, company, category, contact frequency
2. Create the note from the template
3. If you have context from meetings or emails, pre-populate the interaction log
4. Confirm creation

### Auto-Update from Meeting Transcripts

When \`meeting-transcript-processor\` runs, it can trigger people-crm updates:
1. Identify all participants in the transcript
2. For each person with a CRM note, update \`last-contact\` date
3. Append key interaction notes from the meeting summary
4. Flag any new people who should get a CRM note

## Categories

Organize people by how you interact with them:

| Category | Contact Frequency | Notes |
|----------|------------------|-------|
| \`direct-report\` | Weekly | Your leadership team |
| \`board\` | Monthly | Board members, advisors |
| \`client\` | Varies | Key clients across divisions |
| \`vendor\` | Monthly | Important vendors and partners |
| \`pastor\` | Weekly | Church relationships |
| \`elder\` | Biweekly | Elder board |
| \`personal\` | Varies | Friends, mentors, family |

## Obsidian Bases Integration

Create a People dashboard using Obsidian Bases:

\`\`\`markdown
# People Dashboard

[base]
source: People
columns:
  - title
  - role
  - company
  - category
  - last-contact
  - relationship-health
  - contact-frequency
filter: type = "person"
sort: last-contact asc
[/base]
\`\`\`

This gives you a sortable, filterable table of all your relationships right in Obsidian.

## Error Handling

- If person note doesn't exist → offer to create it
- If \`last-contact\` is missing → check Gmail and Calendar for most recent interaction, set it
- If vault path isn't set → ask user for \`$OBSIDIAN_VAULT\`
- If multiple people share a name → ask for clarification, use full name in filename

## Security Rules

- Person notes may contain sensitive personal info — never share externally
- Don't auto-create notes for people unless Josh confirms
- Personal notes (family, health, etc.) stay in Obsidian only — never include in emails or messages

## See Also

- [obsidian-markdown](../obsidian-markdown/SKILL.md) — Note creation and editing
- [obsidian-bases](../obsidian-bases/SKILL.md) — People dashboard views
- [meeting-transcript-processor](../meeting-transcript-processor/SKILL.md) — Auto-log interactions from meetings
- [delegation-tracker](../delegation-tracker/SKILL.md) — Track who owes you what
- [che-email-composer](../che-email-composer/SKILL.md) — Draft outreach messages
