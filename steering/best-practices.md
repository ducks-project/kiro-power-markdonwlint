# Markdown Writing Best Practices

This guide covers writing best practices that go beyond technical linting. These practices ensure readability, accessibility, and maintainability of Markdown documents.

## Language Consistency

### Rule

A Markdown document must be written in a **single language** throughout. Mixing languages within the same document harms readability and comprehension.

### Detection Strategy

Analyze the textual content of the document (excluding code blocks) and identify the dominant language. Flag any passages written in a different language.

### Accepted Exceptions

The following do not constitute language mixing:

- **Technical terms without established translations**: API, framework, commit, push, pull request, deploy, build, debug, refactor, endpoint, middleware, callback, hook, plugin, linting, merge, branch
- **Code blocks**: source code can be in any language
- **Proper nouns and brand names**: GitHub, Docker, Kubernetes, VS Code, Node.js
- **Quoted references** in their original language (within quotation marks)
- **URLs and file paths**
- **Industry-specific domain terms** commonly used in their original language

### Examples

**Correct (consistent English):**

```markdown
# Installation Guide

This document explains how to install and configure the project.

## Prerequisites

- Node.js version 18 or higher
- A GitHub account with repository access
```

**Incorrect (mixed languages):**

```markdown
# Installation Guide

Cette section explains how to installer the project.

## Prerequisites

- Node.js version 18 ou supérieure
- Un GitHub account with access au repo
```

## Fenced Code Blocks with Language Identifier

### Rule

All fenced code blocks (triple backticks) must specify the language used. This enables syntax highlighting and improves readability. Corresponds to markdownlint rule **MD040**.

### Common Language Identifiers

| Language | Identifier(s) |
| -------- | ------------- |
| JavaScript | `javascript`, `js` |
| TypeScript | `typescript`, `ts` |
| Python | `python`, `py` |
| Bash / Shell | `bash`, `sh`, `shell` |
| JSON | `json` |
| YAML | `yaml`, `yml` |
| HTML | `html` |
| CSS | `css` |
| SQL | `sql` |
| Markdown | `markdown`, `md` |
| PHP | `php` |
| Ruby | `ruby`, `rb` |
| Go | `go` |
| Rust | `rust` |
| Java | `java` |
| C# | `csharp`, `cs` |
| Plain text | `text`, `plaintext` |
| Diff | `diff` |
| Configuration | `ini`, `toml` |
| Docker | `dockerfile` |

### Plain Text and Terminal Output

If the block contains plain text, terminal output, or content without a specific language, use `text`:

````markdown
```text
This is an example of terminal output
without any particular syntax highlighting.
```
````

For shell commands with their output, use `console`:

````markdown
```console
$ npm install @ducks-project/markdownlint-mcpserver
added 42 packages in 3s
```
````

## Table of Contents

### Rule

Documents containing **more than 5 sections** (level-2 or deeper headings) should include a table of contents.

### Placement

The table of contents should be placed:

- After the main title (level-1 heading)
- Before the first content section
- Optionally preceded by a brief introduction (1-2 sentences)

### Recommended Format

```markdown
# Document Title

Brief introduction to the document.

## Table of Contents

- [Section 1](#section-1)
- [Section 2](#section-2)
  - [Subsection 2.1](#subsection-21)
  - [Subsection 2.2](#subsection-22)
- [Section 3](#section-3)
- [Section 4](#section-4)
- [Section 5](#section-5)
- [Section 6](#section-6)

## Section 1

Content...
```

### Exceptions

A table of contents is not required for:

- Short README files
- Changelog files (CHANGELOG.md)
- Documented configuration files
- Documents with an obvious flat structure (FAQ, glossary)

### Maintenance

When a document is modified (sections added, removed, or renamed), the table of contents must be updated accordingly.

## Image Alt Text

### Rule

All images must have **descriptive, meaningful alt text**. This is essential for accessibility (screen readers) and for context when images fail to load. Corresponds to markdownlint rule **MD045**.

### Writing Good Alt Text

1. **Describe the content** of the image, not its nature
2. **Be concise** but informative (one short sentence)
3. **Avoid prefixes** like "Image of..." or "Photo of..."
4. **Match the document context** — the description should make sense in context

### Examples

```markdown
<!-- Good -->
![OAuth2 authentication sequence diagram](auth-flow.png)
![Dashboard showing performance metrics for the last 7 days](dashboard.png)
![Markdownlint project logo](logo.svg)

<!-- Bad -->
![](image.png)
![image](screenshot.png)
![photo](photo.jpg)
![alt text](diagram.png)
![click here](button.png)
```

### Decorative Images

For purely decorative images that convey no information, use HTML with an empty alt attribute and aria-hidden:

```html
<img src="decoration.png" alt="" aria-hidden="true" />
```

## Document Structure

### Heading Hierarchy

- One level-1 heading (H1) per document — this is the document title
- Never skip heading levels (no H1 followed by H3 without H2)
- Headings describe the logical structure of the document

### Paragraphs and Spacing

- Separate paragraphs with a single blank line
- No multiple consecutive blank lines
- One blank line before and after headings, lists, code blocks, and tables

### Links

- Use descriptive link text: `[official documentation](url)` rather than `[click here](url)`
- For long or repeated URLs, use reference-style links:

```markdown
See the [official documentation][docs] and the [getting started guide][guide].

[docs]: https://example.com/documentation
[guide]: https://example.com/getting-started
```

### Lists

- Use a consistent marker style (dashes `-` or asterisks `*`, do not mix)
- Indent sub-lists by 2 spaces
- Add a blank line before and after each list

## Review Process

When reviewing a Markdown document, follow this order:

1. **Technical linting** — Run `lint_markdown` and fix auto-fixable violations with `fix_markdown`
2. **Structure** — Verify heading hierarchy and presence of a TOC if needed
3. **Language consistency** — Ensure a single language is used throughout
4. **Code blocks** — Verify all fenced blocks specify a language
5. **Images** — Verify alt text is present and descriptive
6. **Links** — Verify link text is descriptive and links are functional
7. **Configuration compliance** — Ensure the review respects the project's configuration file
