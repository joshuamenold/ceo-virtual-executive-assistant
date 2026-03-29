---
name: writing-plans
description: "Structure long-form writing projects with outlines, drafts, and iterative refinement. Use this skill when the user wants to plan a blog post, article, book chapter, speech, or any long-form written content."
---

# Writing Plans — Long-Form Content Structure

Help the user plan and structure long-form writing projects — from blog posts to book chapters.

## Workflow

### Step 1: Understand the Project

Ask the user:
- **What** are you writing? (blog post, article, book chapter, speech, memo)
- **Who** is the audience? (board, customers, team, public)
- **What's the goal?** (inform, persuade, inspire, document)
- **How long?** (500 words, 2000 words, full chapter)
- **Any constraints?** (deadline, style guide, tone)

### Step 2: Create an Outline

Structure the piece with a clear hierarchy:
```markdown
# [Title]

## Hook / Opening
- Attention-grabbing opening line or story
- Why this matters to the reader

## Context / Background
- Set the scene
- Define the problem or opportunity

## Main Argument / Body
### Point 1: [Key insight]
- Supporting evidence
- Example or anecdote

### Point 2: [Key insight]
- Supporting evidence
- Example or anecdote

### Point 3: [Key insight]
- Supporting evidence
- Example or anecdote

## Implications / So What?
- What this means for the reader
- What should change as a result

## Call to Action / Closing
- Clear next step
- Memorable closing line
```

### Step 3: Draft Section by Section

Write each section one at a time:
1. Start with the section the user feels strongest about (not necessarily the intro)
2. Get feedback on each section before moving to the next
3. Save drafts to Obsidian as you go

### Step 4: Revise and Polish

- Read the full draft aloud (or suggest the user does)
- Check for flow between sections
- Tighten the opening — cut anything before the real start
- Strengthen the closing — end with impact, not a whimper
- Cut ruthlessly — if a sentence doesn't earn its place, remove it

## Templates by Format

### Blog Post (800-1500 words)
```
Hook (1 paragraph) → Context (1-2 paragraphs) → 3 Main Points (2-3 paragraphs each) → Takeaway (1 paragraph) → CTA (1 sentence)
```

### Executive Memo (500-1000 words)
```
Bottom Line Up Front → Background → Analysis → Recommendation → Next Steps
```

### Book Chapter (3000-5000 words)
```
Opening Story → Chapter Thesis → 3-5 Major Sections (each with sub-points, stories, evidence) → Chapter Summary → Bridge to Next Chapter
```

### Speech (10-20 minutes)
```
Opening Hook → "Why we're here" → 3 Key Messages (with stories) → Call to Action → Memorable Close
```

## Saving to Obsidian

Save outlines and drafts to the vault:
```bash
cat > "$OBSIDIAN_VAULT/Writing/[Project Name] - Outline.md" << 'EOF'
---
title: "[Project Name] - Outline"
date: 2026-03-28
tags: [writing, draft]
status: outline
project: project-name
---

[Outline content here]
EOF
```

## Tips

- The outline is the most important step — spend time getting the structure right before writing
- Write the first draft fast and messy, then revise carefully
- Every section should answer "so what?" for the reader
- Use stories and examples to make abstract points concrete
- The best writing has a strong point of view — don't hedge everything

## See Also

- [obsidian-markdown](../obsidian-markdown/SKILL.md) — Save drafts to vault
- [tavily-search](../tavily-search/SKILL.md) — Research for content
