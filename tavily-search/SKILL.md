---
name: tavily-search
description: "Search the web for current information using Tavily. Use this skill when the user wants to research something, look up current news, find information about a competitor, get market data, or answer any question that requires up-to-date web information."
---

# Tavily — Web Search

## Prerequisites

Set the `TAVILY_API_KEY` environment variable:
```bash
export TAVILY_API_KEY="tvly-your-key-here"
```

Get a free API key at [tavily.com](https://tavily.com). If the key isn't set, tell the user to sign up and set the variable.

## Basic Search
```bash
curl -s -X POST https://api.tavily.com/search \
  -H "Content-Type: application/json" \
  -d "{
    \"api_key\": \"$TAVILY_API_KEY\",
    \"query\": \"your search query here\",
    \"search_depth\": \"basic\",
    \"max_results\": 5
  }" | jq '.results[] | {title, url, content}'
```

## Deep Search (more thorough, slower)
```bash
curl -s -X POST https://api.tavily.com/search \
  -H "Content-Type: application/json" \
  -d "{
    \"api_key\": \"$TAVILY_API_KEY\",
    \"query\": \"your search query here\",
    \"search_depth\": \"advanced\",
    \"max_results\": 10
  }" | jq '.results[] | {title, url, content}'
```

## Search with Domain Filtering
```bash
# Only search specific domains
curl -s -X POST https://api.tavily.com/search \
  -H "Content-Type: application/json" \
  -d "{
    \"api_key\": \"$TAVILY_API_KEY\",
    \"query\": \"AI regulation 2026\",
    \"include_domains\": [\"reuters.com\", \"nytimes.com\", \"wsj.com\"],
    \"max_results\": 5
  }" | jq '.results[] | {title, url, content}'
```

## Search Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | required | The search query |
| `search_depth` | string | `basic` | `basic` (fast) or `advanced` (thorough) |
| `max_results` | int | 5 | Number of results (1-20) |
| `include_domains` | array | — | Only search these domains |
| `exclude_domains` | array | — | Exclude these domains |
| `include_answer` | bool | false | Include an AI-generated summary |

## How to Present Search Results

Don't just dump raw JSON. Summarize the findings:
```
Search: "competitor product launch Q2 2026"

Here's what I found:

1. Acme Corp Launches New Platform (reuters.com, Mar 25)
   They announced a new enterprise platform targeting mid-market companies...

2. Industry Analysis: Q2 Product Launches (techcrunch.com, Mar 22)
   Overview of major launches this quarter, including...

Key takeaway: The main competitive threat is Acme's new mid-market play,
which directly overlaps with your Q2 expansion plans.
```

## Tips

- All searches are read-only and safe to run without confirmation
- Use `basic` depth for quick lookups, `advanced` for research tasks
- Use `include_domains` when the user wants news from specific sources
- Always synthesize and summarize results — don't just list URLs
- Combine with Obsidian skills to save research findings as notes

## See Also

- [obsidian-markdown](../obsidian-markdown/SKILL.md) — Save research as notes
- [recipe-daily-briefing](../recipe-daily-briefing/SKILL.md) — Include web research in briefings
