---
name: gws-shared
description: "Shared reference for all Google Workspace skills. Contains authentication, global flags, security rules, and CLI syntax for the gws CLI. Read this skill before using any gws-* skill."
---

# gws — Shared Reference

All Google Workspace skills in this stack use the `gws` CLI. Read this before using any `gws-*` skill.

## Installation

The `gws` binary must be on `$PATH`. Install it:
```bash
curl -fsSL https://raw.githubusercontent.com/googleworkspace/cli/main/install.sh | bash
```

## Authentication
```bash
# Browser-based OAuth (interactive — run this in the user's terminal)
gws auth login

# Or use a service account
export GOOGLE_APPLICATION_CREDENTIALS=/path/to/key.json
```

If any gws command returns an auth error, tell the user to run `gws auth login`.

## CLI Syntax
```bash
gws <service> <resource> <method> [flags]
```

## Global Flags

| Flag | Description |
|------|-------------|
| `--format <FORMAT>` | Output format: `json` (default), `table`, `yaml`, `csv` |
| `--dry-run` | Validate locally without calling the API |

## Method Flags

| Flag | Description |
|------|-------------|
| `--params '{"key": "val"}'` | URL/query parameters |
| `--json '{"key": "val"}'` | Request body |
| `-o, --output <PATH>` | Save binary responses to file |
| `--upload <PATH>` | Upload file content (multipart) |
| `--page-all` | Auto-paginate (NDJSON output) |

## Discovering Commands

Before calling any API method, inspect it first:
```bash
# Browse available resources and methods
gws gmail --help

# Inspect a method's required params, types, and defaults
gws schema gmail.<resource>.<method>
```

Always use `gws schema` to check required parameters before building a command.

## Security Rules

- **Never** output secrets, API keys, or tokens directly
- **Always** confirm with the user before executing any write or delete command
- Prefer `--dry-run` for destructive operations
- Read operations (list, get, triage, agenda) are always safe to run without confirmation

## Shell Tips

- **zsh `!` expansion:** Ranges like `Sheet1!A1` contain `!` which zsh interprets as history expansion. Use double quotes:
```bash
  gws sheets +read --spreadsheet ID --range "Sheet1\!A1:D10"
```
- **JSON with double quotes:** Wrap `--params` and `--json` in single quotes:
```bash
  gws drive files list --params '{"pageSize": 5}'
```
