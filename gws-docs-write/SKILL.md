---
name: gws-docs-write
description: "Google Docs: Create and edit documents. Use this skill when the user wants to create a new Google Doc, write content to a doc, edit an existing document, or format a doc."
---

# Google Docs — Write and Edit

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.

## Create a New Document
```bash
gws docs documents create --json '{"title": "Q2 Strategy Brief"}'
```

This returns the new document ID. Use it for subsequent edits.

## Edit Document Content

Use the `batchUpdate` method to insert, delete, or format text:
```bash
# Insert text at the beginning
gws docs documents batchUpdate --params '{"documentId": "DOC_ID"}' --json '{
  "requests": [
    {
      "insertText": {
        "location": {"index": 1},
        "text": "Executive Summary\n\nOur Q2 results show..."
      }
    }
  ]
}'
```

### Common batchUpdate Operations

**Insert text at a position:**
```json
{"insertText": {"location": {"index": 1}, "text": "Hello World"}}
```

**Delete text range:**
```json
{"deleteContentRange": {"range": {"startIndex": 1, "endIndex": 50}}}
```

**Format text as heading:**
```json
{"updateParagraphStyle": {"range": {"startIndex": 1, "endIndex": 20}, "paragraphStyle": {"namedStyleType": "HEADING_1"}, "fields": "namedStyleType"}}
```

## Tips

- Get the document first (`gws docs documents get`) to see current content and indices before editing.
- The index is 1-based (index 1 is the start of the document body).
- Batch multiple operations in a single `batchUpdate` call for efficiency.

> **WRITE COMMAND:** Confirm with the user before creating or modifying documents.

## See Also

- [gws-shared](../gws-shared/SKILL.md) — Global flags and auth
- [gws-docs](../gws-docs/SKILL.md) — Read documents
- [gws-drive](../gws-drive/SKILL.md) — Find files in Drive
