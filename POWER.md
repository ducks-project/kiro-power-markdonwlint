---
name: "markdownlint"
displayName: "Markdownlint"
description: "Lint, validate, and auto-fix Markdown files using markdownlint rules. Enforces writing best practices including language consistency, fenced code language tags, image alt text, and table of contents for long documents."
keywords: ["markdownlint", "markdown", "linting", "formatting", "commonmark"]
author: "Adrien Loyant"
---

# Markdownlint

## Overview

This power provides comprehensive Markdown quality enforcement through the [`@ducks-project/markdownlint-mcpserver`](https://github.com/ducks-project/markdownlint-mcpserver) package, an MCP server built on the [markdownlint](https://github.com/DavidAnson/markdownlint) library. It can analyze, validate, and auto-fix `.md` files against all 52 official markdownlint rules, lint files by path with automatic config discovery, and recursively scan directories or glob patterns.

Beyond technical linting, this power enforces writing best practices: language consistency within documents, fenced code blocks with language identifiers, descriptive alt text on images, and table of contents for long documents.

## Available Steering Files

- **rules-reference** — Complete reference of all 52 markdownlint rules with fixability status, categories, and configuration examples
- **best-practices** — Writing best practices beyond linting: language consistency, accessibility, document structure

## Onboarding

When working on a project with Markdown files, perform these setup checks on first interaction:

### 1. VS Code / Kiro Extension

Check whether the file `.vscode/extensions.json` exists and contains `"davidanson.vscode-markdownlint"` in its `recommendations` array.

If the extension is not listed, suggest creating or updating `.vscode/extensions.json`:

```json
{
  "recommendations": [
    "davidanson.vscode-markdownlint"
  ]
}
```

If the file already exists, only append the identifier to the existing `recommendations` array.

**Do not suggest this more than once per session.** If the user declines, respect the decision and do not ask again.

### 2. Project Configuration File

Check whether a markdownlint configuration file exists at the project root. Recognized files (in priority order):

1. `.markdownlint-cli2.jsonc`
2. `.markdownlint.jsonc`
3. `.markdownlint.json`
4. `.markdownlint.yaml` / `.markdownlint.yml`

If none exists, suggest creating `.markdownlint.json` with this starter configuration:

```json
{
  "default": true,
  "MD013": false,
  "MD024": {
    "siblings_only": true
  },
  "MD033": {
    "allowed_elements": ["a", "br", "details", "summary", "sup", "sub"]
  },
  "MD040": true,
  "MD045": true
}
```

Rationale:

- Enables all rules by default
- Disables MD013 (line length) as it conflicts with natural prose writing
- Only check sibling headings for multiple headings with the same content
- Allows common HTML elements via MD033
- Enforces language specification in fenced code blocks (MD040)
- Requires alt text on images (MD045)

**Do not suggest this more than once.** If the user has already accepted or declined, do not re-propose.

### 3. Configuration File Authority

The project's markdownlint configuration file is the **single source of truth**. When a configuration file exists:

- Respect all enabled/disabled rules as defined
- Never contradict custom parameters set by the project
- Only report violations for rules that are actually active
- Adapt fix suggestions to match the project's configuration
- If a rule is disabled in config, do not flag it even if it would normally be a best practice

## Common Workflows

### Lint a Markdown File

1. Read the target Markdown file
2. Check whether a markdownlint configuration file exists in the project
3. Use `lint_markdown` with `filePath` to analyze the file (auto-discovers project config)
4. Present results grouped by category (formatting, structure, links, etc.)
5. If auto-fixable violations exist, offer to run `fix_markdown`

### Lint a Directory or Glob Pattern

1. Use `lint_directory` with a `path` (and optional `glob` pattern) to scan multiple files
2. Review the summary: files scanned, files with issues, total issues
3. Present per-file results grouped by severity
4. Offer to fix individual files with `fix_markdown`

### Auto-fix a Markdown File

1. Use `fix_markdown` on the file content to apply automatic corrections (covers ~30 of 52 rules)
2. Compare the output with the original to identify changes
3. Apply the corrected content to the file
4. Re-run `lint_markdown` to check for remaining issues (non-fixable rules)
5. Provide manual fix suggestions for remaining violations

### Check Active Configuration

1. Use `get_configuration` to display the current linting ruleset
2. Compare with the project's configuration file if one exists
3. Report any discrepancies

### Full Document Review

Perform a comprehensive quality review combining technical linting and writing best practices:

1. Run `lint_markdown` for technical violations
2. Check language consistency — no mixing of languages within a document (e.g., French and English mixed together)
3. Verify all fenced code blocks specify a language identifier
4. Verify all images have descriptive alt text
5. If the document has more than 5 level-2+ headings, check for a table of contents
6. Produce a consolidated report with priorities

## Writing Best Practices

These practices complement technical linting and should be enforced during reviews:

### Language Consistency

A Markdown document must be written in a single language throughout. Detect and flag language mixing in the same document. Accepted exceptions:

- Technical terms without established translations (API, framework, commit, deploy, etc.)
- Code blocks (code can be in any language)
- Proper nouns and brand names
- Quoted references in their original language

### Fenced Code Blocks with Language

All fenced code blocks (triple backticks) must specify a language. This corresponds to rule MD040. If the content is plain text or terminal output, use `text` or `console`.

### Table of Contents

Documents with more than 5 sections (level-2 or deeper headings) should include a table of contents after the main title and before the first content section.

### Image Alt Text

All images must have descriptive, meaningful alt text — not empty, not generic placeholders like "image" or "screenshot". This corresponds to rule MD045.

## Troubleshooting

### MCP Server Won't Start

**Symptoms:** Connection error, server not responding

**Solutions:**

1. Verify Node.js is installed: `node --version` (requires 18+)
2. Verify npx is available: `npx --version`
3. Test manually: `npx -y @ducks-project/markdownlint-mcpserver@latest`
4. If behind a proxy, ensure npm proxy settings are configured
5. If npx caching issues, clear the cache: `npx --package @ducks-project/markdownlint-mcpserver@latest markdownlint-mcpserver`

### Rules Don't Match Project Configuration

**Symptoms:** Linting reports violations for rules disabled in the project config

**Solutions:**

1. Run `get_configuration` to see the active ruleset
2. Confirm the configuration file is at the project root
3. Validate JSON/YAML syntax of the configuration file
4. Note: `.markdownlint-cli2.jsonc` uses a different schema than `.markdownlint.json`

### Some Violations Are Not Auto-fixable

22 of 52 rules require human judgment and cannot be fixed automatically. Common examples:

- Heading structure (MD001, MD024, MD025)
- Line length (MD013)
- Code block style (MD046)
- Intentional inline HTML (MD033)

For these cases, provide a diagnosis and suggest corrections, but the user must apply them manually.

## Best Practices

- Always check for a project configuration file before linting
- The project configuration file takes precedence over defaults
- Run `fix_markdown` first, then address remaining issues manually
- Group corrections by type for cleaner diffs
- Do not mix languages within a document
- Always specify a language for fenced code blocks
- Add a table of contents for long documents (> 5 sections)
- Write descriptive alt text for all images

## Configuration

**No additional configuration required** — the MCP server is fetched automatically via npx on first launch. Only Node.js 18+ is needed.

**Optional project configuration files:**

- `.markdownlint.json` — Rule configuration in JSON format
- `.markdownlint.jsonc` — Same with comment support
- `.markdownlint.yaml` / `.markdownlint.yml` — Rule configuration in YAML format
- `.markdownlint-cli2.jsonc` — Configuration for markdownlint-cli2

### Configuration Merge Hierarchy (lowest to highest priority)

1. Default config (`{ "default": true, "MD013": false }`)
2. Filesystem config (nearest `.markdownlint.json` / `.yaml` / `.jsonc` discovered from the file's directory)
3. Runtime config (passed in tool `config` parameter)

---

**MCP Server:** [`@ducks-project/markdownlint-mcpserver`](https://github.com/ducks-project/markdownlint-mcpserver)
**Linting engine:** [markdownlint](https://github.com/DavidAnson/markdownlint)
**VS Code Extension:** `davidanson.vscode-markdownlint`
**Rules Reference:** <https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md>
