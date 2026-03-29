---
name: gws-drive
description: "Google Drive: Browse and manage files and folders. Use this skill when the user wants to find a file, list drive contents, upload or download files, or organize their Drive."
---

# Google Drive — File Management

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.
```bash
gws drive <resource> <method> [flags]
```

## Common Operations

### List recent files
```bash
gws drive files list --params '{"pageSize": 10, "orderBy": "modifiedTime desc", "fields": "files(id,name,mimeType,modifiedTime)"}'
```

### Search for files by name
```bash
gws drive files list --params '{"q": "name contains '\''budget'\''", "fields": "files(id,name,mimeType,modifiedTime)"}'
```

### Search by type
```bash
# Google Docs only
gws drive files list --params '{"q": "mimeType='\''application/vnd.google-apps.document'\''", "fields": "files(id,name)"}'

# Spreadsheets only
gws drive files list --params '{"q": "mimeType='\''application/vnd.google-apps.spreadsheet'\''", "fields": "files(id,name)"}'

# PDFs only
gws drive files list --params '{"q": "mimeType='\''application/pdf'\''", "fields": "files(id,name)"}'
```

### Download a file
```bash
gws drive files get --params '{"fileId": "FILE_ID", "alt": "media"}' -o downloaded_file.pdf
```

### Upload a file
```bash
gws drive +upload --file report.pdf --name 'Q2 Report' --folder 'FOLDER_ID'
```

### List files in a specific folder
```bash
gws drive files list --params '{"q": "'\''FOLDER_ID'\'' in parents", "fields": "files(id,name,mimeType)"}'
```

## Drive Search Query Syntax

| Query | What it finds |
|-------|-------------|
| `name contains 'budget'` | Files with "budget" in the name |
| `mimeType='application/pdf'` | PDF files |
| `modifiedTime > '2026-03-01'` | Modified after March 1 |
| `'FOLDER_ID' in parents` | Files in a specific folder |
| `trashed = false` | Non-trashed files |
| `sharedWithMe` | Files shared with you |

## Tips

- Always include `fields` parameter to limit response size.
- Use `trashed=false` to exclude deleted files from results.
- Read operations (list, get, search) are safe to run without confirmation.
- Upload and delete operations require user confirmation.

## See Also

- [gws-shared](../gws-shared/SKILL.md) — Global flags and auth
- [gws-docs](../gws-docs/SKILL.md) — Read Google Docs
- [gws-docs-write](../gws-docs-write/SKILL.md) — Write Google Docs
