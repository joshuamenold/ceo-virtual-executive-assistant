---
name: quick-capture
description: "Quickly capture a thought, idea, task, or note and route it to the right place in Obsidian without specifying templates or structure. Use this skill when the user dumps a quick thought, says 'capture this', 'note to self', 'remind me', 'jot this down', 'save this thought', 'add to my vault', or drops a short piece of text that needs to go somewhere. Also trigger when the user shares a Bible verse, sermon idea, book idea, business insight, or any quick snippet that should be saved but doesn't warrant a full structured note."
---

# Quick Capture — Thought-to-Vault in Seconds

Capture a thought and route it to the right place in Obsidian. No template selection, no structure decisions — just capture and go.

## Why This Matters

CEOs have ideas at random moments. The best ones get lost because writing a structured note feels like too much friction. This skill eliminates friction — you say it, it gets saved in the right place, and you move on.

## Routing Logic

Based on content analysis, route to the correct folder and format:

| Content Type | Destination | Format |
|-------------|-------------|--------|
| Task / to-do | Daily Note → Action Items | \`- [ ] [task]\` |
| Business idea | \`Ideas/Business/\` | Idea note |
| Book idea / chapter thought | \`Writing/Ideas/\` | Writing idea note |
| Sermon idea / Bible verse | \`Faith/\` | Faith note |
| Meeting follow-up | Daily Note → Follow-ups | Bullet under today's date |
| Person-related note | \`People/[Name].md\` | Append to Notes section |
| Decision or realization | \`Decision Log/\` | Decision entry |
| Random thought | \`Inbox/\` | Quick note |

## Workflow

### Step 1: Receive the Capture

User says something like:
- "Note to self: ask Sarah about the vendor contract renewal"
- "Capture this — I think we should pivot the Cornerstone marketing to focus on 3-season rooms"
- "Jot down Philippians 4:13 — use in Sunday's sermon"
- "Remind me to call Tom about the board meeting agenda"
- "Save this: the Q2 revenue target should be $2.4M not $2.2M"

### Step 2: Classify and Route

Analyze the content to determine:
1. **Type**: task, idea, note, verse, follow-up, decision
2. **Destination**: which folder/file in the vault
3. **Tags**: auto-tag based on content
4. **Urgency**: is this time-sensitive?

### Step 3: Save to Obsidian

#### Quick Task
\`\`\`bash
# Append to today's daily note
cat >> "$OBSIDIAN_VAULT/Daily Notes/$(date +%Y-%m-%d).md" << 'EOF'

## Captured Tasks
- [ ] [task description] #captured
EOF
\`\`\`

#### Business Idea
\`\`\`bash
cat > "$OBSIDIAN_VAULT/Ideas/Business/$(date +%Y-%m-%d) - [Brief Title].md" << 'EOF'
---
title: "[Brief Title]"
date: [today]
tags: [idea, business, captured]
type: idea
status: captured
---

# [Brief Title]

[The captured thought, expanded slightly for future context]

## Initial Thinking
[Any additional context the user provided]

## Next Steps
- [ ] Think through this further
- [ ] Discuss with [relevant person]
EOF
\`\`\`

#### Sermon / Faith Note
\`\`\`bash
cat > "$OBSIDIAN_VAULT/Faith/$(date +%Y-%m-%d) - [Brief Title].md" << 'EOF'
---
title: "[Brief Title]"
date: [today]
tags: [faith, captured]
type: faith-note
---

# [Brief Title]

[Verse or thought]

## Context
[Why this stood out, how to use it]
EOF
\`\`\`

#### Book / Writing Idea
\`\`\`bash
cat > "$OBSIDIAN_VAULT/Writing/Ideas/$(date +%Y-%m-%d) - [Brief Title].md" << 'EOF'
---
title: "[Brief Title]"
date: [today]
tags: [writing, idea, captured]
type: writing-idea
book: "[which book if known]"
---

# [Brief Title]

[The captured thought]

## How This Fits
[Which chapter or section this relates to, if obvious]
EOF
\`\`\`

#### Person Note
\`\`\`bash
# Append to existing person note
cat >> "$OBSIDIAN_VAULT/People/[Name].md" << 'EOF'

### [Date] — Quick Note
- [The captured note]
EOF
\`\`\`

#### Inbox (when type is unclear)
\`\`\`bash
cat > "$OBSIDIAN_VAULT/Inbox/$(date +%Y-%m-%d) - [Brief Title].md" << 'EOF'
---
title: "[Brief Title]"
date: [today]
tags: [inbox, captured]
type: quick-capture
---

# [Brief Title]

[The captured thought]
EOF
\`\`\`

### Step 4: Confirm

Keep confirmation ultra-brief:
> "Saved to [location]. Tagged #[tags]."

That's it. No long explanation. The user is in capture mode, not review mode.

## Batch Capture

If the user dumps multiple items at once:
1. Parse each distinct thought
2. Route each separately
3. Confirm with a summary:

\`\`\`
Captured 4 items:
  ✓ Task → Daily Note: "Call Tom about board agenda"
  ✓ Idea → Business: "Pivot Cornerstone marketing to 3-season rooms"
  ✓ Task → Daily Note: "Review Q2 budget draft"
  ✓ Faith → Faith: "Philippians 4:13 sermon note"
\`\`\`

## Also Create Asana Task (Optional)

If the captured item is a task and seems important enough:
1. Ask: "Want me to create an Asana task for this too?"
2. If yes, use \`create_task_preview\` with appropriate project and due date
3. Link the Asana task in the Obsidian note

## Smart Defaults

- If no date/deadline mentioned → no due date (don't invent urgency)
- If no person mentioned → don't assign
- If category is ambiguous → use Inbox (better to capture than to over-categorize)
- If the vault folder doesn't exist → create it
- Always add \`#captured\` tag so items can be reviewed later

## Error Handling

- If Obsidian vault path isn't set → ask user, then set \`$OBSIDIAN_VAULT\`
- If daily note doesn't exist for today → create it with standard template
- If person note doesn't exist → save to Inbox, mention "no person note found for [Name]"
- If content is too vague to classify → save to Inbox, tag as \`#needs-review\`

## See Also

- [obsidian-markdown](../obsidian-markdown/SKILL.md) — Full note creation
- [obsidian-cli](../obsidian-cli/SKILL.md) — Vault search and management
- [people-crm](../people-crm/SKILL.md) — Person note updates
- [asana](../asana/SKILL.md) — Task creation from captures
