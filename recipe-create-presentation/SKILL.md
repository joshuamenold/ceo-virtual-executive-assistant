---
name: recipe-create-presentation
description: "Generate Google Slides presentations from outlines or topics. Use this skill when the user wants to create a slide deck, build a presentation, prepare slides for a meeting, or make a pitch deck."
---

# Recipe: Create Presentation

Generate Google Slides presentations using the Google Slides API via `gws`.

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.

## How It Works

### Step 1: Create a blank presentation
```bash
gws slides presentations create --json '{"title": "Q2 Board Update"}'
```

Save the `presentationId` from the response.

### Step 2: Add slides with content
```bash
gws slides presentations batchUpdate --params '{"presentationId": "PRESENTATION_ID"}' --json '{
  "requests": [
    {
      "createSlide": {
        "slideLayoutReference": {"predefinedLayout": "TITLE_AND_BODY"},
        "insertionIndex": 1
      }
    }
  ]
}'
```

### Step 3: Add text to slides
```bash
gws slides presentations batchUpdate --params '{"presentationId": "PRESENTATION_ID"}' --json '{
  "requests": [
    {
      "insertText": {
        "objectId": "TITLE_PLACEHOLDER_ID",
        "text": "Q2 Revenue Results"
      }
    },
    {
      "insertText": {
        "objectId": "BODY_PLACEHOLDER_ID",
        "text": "Revenue up 15% YoY\n3 new enterprise clients\nExpansion into APAC market"
      }
    }
  ]
}'
```

## Slide Layout Options

| Layout | Best for |
|--------|----------|
| `TITLE` | Title slide (first slide) |
| `TITLE_AND_BODY` | Standard content slide |
| `TITLE_AND_TWO_COLUMNS` | Comparison or side-by-side |
| `SECTION_HEADER` | Section divider |
| `BLANK` | Custom layouts, images |

## Recommended Deck Structure for a CEO

### Board Update (8-10 slides)

1. **Title Slide** — Company Name, Date, Presenter
2. **Executive Summary** — 3 bullet overview
3. **Financial Highlights** — Revenue, margins, cash position
4. **Key Metrics** — KPIs with trends
5. **Product Update** — Launches, roadmap
6. **Team and Hiring** — Headcount, key hires
7. **Risks and Challenges** — Top 3 risks with mitigation
8. **Strategic Priorities** — Next quarter focus
9. **Ask / Discussion** — What you need from the board

### Investor Pitch (10-12 slides)

1. Title  2. Problem  3. Solution  4. Market Size  5. Product  6. Traction  7. Business Model  8. Competition  9. Team  10. Financials  11. Ask  12. Appendix

## Workflow

1. Ask the user for the topic and audience
2. Draft an outline (show to user for approval)
3. Create the presentation and populate slides
4. Share the Google Slides link so the user can review and polish visually

## Tips

- Start with the outline — get agreement on structure before creating slides
- Keep slides minimal (5-7 bullets max per slide)
- The Slides API is complex — for simple decks, it may be easier to create a Google Doc outline and convert it
- Always provide the shareable link at the end

> **WRITE COMMAND:** Confirm the outline and structure with the user before creating.

## Skills Used

- [gws-shared](../gws-shared/SKILL.md)
- [gws-drive](../gws-drive/SKILL.md)
