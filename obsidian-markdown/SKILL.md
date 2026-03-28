---
name: obsidian-markdown
preamble-tier: 3
version: 1.0.0
description: Create and edit Obsidian notes with proper markdown formatting. Handles frontmatter, links, tags, and Obsidian-specific syntax.
allowed-tools:
  - Read
    - Write
      - Edit
        - Glob
        ---

        # Obsidian Markdown

        Create and edit notes in an Obsidian vault using proper markdown conventions.

        ## Key Principles

        - Use standard markdown with Obsidian extensions (wikilinks, callouts, frontmatter)
        - Always include YAML frontmatter with date, tags, and relevant metadata
        - Use `[[wikilinks]]` for internal note references
        - Use `#tags` for categorization
        - Follow the user's existing vault structure and naming conventions

        ## File Structure

        - Notes should be saved as `.md` files in the appropriate vault directory
        - Use descriptive filenames with no special characters
        - Respect folder hierarchy if one exists

        ## Formatting Guidelines

        - Use headings (H1-H6) for document structure
        - Use bullet lists for unordered items, numbered lists for sequences
        - Use callouts (`> [!note]`, `> [!warning]`, etc.) for highlighted content
        - Use code blocks with language identifiers for code
        - Use `---` for horizontal rules between sections