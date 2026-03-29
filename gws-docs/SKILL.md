---
name: gws-docs
description: "Google Docs: Read document content. Use this skill when the user wants to read, review, or access a Google Doc, or when they reference a doc by name or URL."
---

# Google Docs — Read

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.
```bash
gws docs <resource> <method> [flags]
```

## Common Operations

### Get document content
```bash
gws docs documents get --params '{"documentId": "DOC_ID"}'
```

### Find a doc by name (via Drive)
```bash
gws drive files list --params '{"q": "name contains '\''quarterly review'\'' and mimeType='\''application/vnd.google-apps.document'\''", "fields": "files(id,name,modifiedTime)"}'
```

### Export document as plain text
```bash
gws drive files export --params '{"fileId": "DOC_ID", "mimeType": "text/plain"}' -o output.txt
```

### Export as PDF
```bash
gws drive files export --params '{"fileId": "DOC_ID", "mimeType": "application/pdf"}' -o output.pdf
```

## Tips

- Document IDs can be extracted from Google Docs URLs: `docs.google.com/document/d/{DOC_ID}/edit`
- Use Drive's search to find docs by name when the user doesn't provide an ID.
- Read operations are safe to run without confirmation.

## See Also

- [gws-shared](../gws-shared/SKILL.md) — Global flags and auth
- [gws-docs-write](../gws-docs-write/SKILL.md) — Create and edit docs
- [gws-drive](../gws-drive/SKILL.md) — Find files in Drive
