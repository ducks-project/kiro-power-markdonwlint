# Markdownlint Rules Reference

Complete reference of all 52 markdownlint rules, organized by category with fixability status.

Official source: <https://github.com/DavidAnson/markdownlint/blob/main/doc/Rules.md>

## Auto-fixable Rules (30 rules)

These rules can be automatically corrected by the `fix_markdown` tool.

### Whitespace and Formatting (9 rules)

| Rule | Alias | Description |
| ---- | ----- | ----------- |
| MD009 | no-trailing-spaces | Trailing spaces at end of line |
| MD010 | no-hard-tabs | Hard tab characters |
| MD012 | no-multiple-blanks | Multiple consecutive blank lines |
| MD022 | blanks-around-headings | Blank lines required around headings |
| MD027 | no-multiple-space-blockquote | Multiple spaces after blockquote symbol |
| MD031 | blanks-around-fences | Blank lines required around fenced code blocks |
| MD032 | blanks-around-lists | Blank lines required around lists |
| MD047 | single-trailing-newline | File must end with a single newline |
| MD058 | blanks-around-tables | Blank lines required around tables |

### Heading Formatting (6 rules)

| Rule | Alias | Description |
| ---- | ----- | ----------- |
| MD018 | no-missing-space-atx | No space after hash in ATX heading |
| MD019 | no-multiple-space-atx | Multiple spaces after hash in ATX heading |
| MD020 | no-missing-space-closed-atx | No space inside closed ATX heading hashes |
| MD021 | no-multiple-space-closed-atx | Multiple spaces inside closed ATX heading hashes |
| MD023 | heading-start-left | Headings must start at beginning of line |
| MD026 | no-trailing-punctuation | Trailing punctuation in heading |

### List Formatting (4 rules)

| Rule | Alias | Description |
| ---- | ----- | ----------- |
| MD004 | ul-style | Unordered list style consistency |
| MD005 | list-indent | List item indentation consistency |
| MD007 | ul-indent | Unordered list indentation |
| MD030 | list-marker-space | Spaces after list markers |

### Links and Text Formatting (7 rules)

| Rule | Alias | Description |
| ---- | ----- | ----------- |
| MD011 | no-reversed-links | Reversed link syntax `(text)[url]` |
| MD034 | no-bare-urls | Bare URL without angle brackets |
| MD037 | no-space-in-emphasis | Spaces inside emphasis markers |
| MD038 | no-space-in-code | Spaces inside code span backticks |
| MD039 | no-space-in-links | Spaces inside link text brackets |
| MD049 | emphasis-style | Emphasis (italic) style consistency |
| MD050 | strong-style | Strong (bold) style consistency |

### Advanced Fixable (4 rules)

| Rule | Alias | Description |
| ---- | ----- | ----------- |
| MD014 | commands-show-output | Unnecessary dollar signs before commands |
| MD044 | proper-names | Proper name capitalization |
| MD051 | link-fragments | Link fragment validation |
| MD053 | link-image-reference-definitions | Unused reference definitions |

## Detection-only Rules (22 rules)

These rules require human judgment and cannot be automatically fixed.

### Structural and Content Rules (12 rules)

| Rule | Alias | Description | Why not fixable |
| ---- | ----- | ----------- | --------------- |
| MD001 | heading-increment | Heading levels should increment by one | Requires understanding document structure |
| MD003 | heading-style | Heading style consistency | Style preference decision |
| MD013 | line-length | Line length limit | Requires content-aware line breaking |
| MD024 | no-duplicate-heading | Multiple headings with same content | Could break document navigation |
| MD025 | single-h1 | Multiple top-level headings | Requires understanding document hierarchy |
| MD028 | no-blanks-blockquote | Blank line inside blockquote | Ambiguous semantic intent |
| MD029 | ol-prefix | Ordered list item prefix style | Style preference decision |
| MD035 | hr-style | Horizontal rule style | Style preference decision |
| MD036 | no-emphasis-as-heading | Emphasis used instead of heading | Requires semantic understanding |
| MD041 | first-line-h1 | First line should be a top-level heading | May break existing structure |
| MD043 | required-headings | Required heading structure | Document-specific requirements |
| MD046 | code-block-style | Code block style (fenced vs indented) | Style preference decision |

### Content and Language Rules (5 rules)

| Rule | Alias | Description | Why not fixable |
| ---- | ----- | ----------- | --------------- |
| MD033 | no-inline-html | Inline HTML elements | May be intentional |
| MD040 | fenced-code-language | Fenced code blocks need a language | Requires code language knowledge |
| MD045 | no-alt-text | Images must have alt text | Requires understanding image content |
| MD059 | descriptive-link-text | Link text should be descriptive | Requires understanding context |
| MD042 | no-empty-links | Empty link destinations | May be a placeholder |

### Reference and Link Rules (3 rules)

| Rule | Alias | Description | Why not fixable |
| ---- | ----- | ----------- | --------------- |
| MD052 | reference-links-images | Reference links/images must be defined | May be external or conditional |
| MD054 | link-image-style | Link and image style consistency | Style preference decision |
| MD056 | table-column-count | Table column count consistency | May be intentional |

### Table Rules (2 rules)

| Rule | Alias | Description | Why not fixable |
| ---- | ----- | ----------- | --------------- |
| MD055 | table-pipe-style | Table pipe character style | Style preference decision |
| MD048 | code-fence-style | Code fence style (backtick vs tilde) | Style preference decision |

### Other Rules

| Rule | Alias | Description | Fixable |
| ---- | ----- | ----------- | ------- |
| MD060 | table-column-style | Table column alignment style | Partial |

## Rule Configuration

### Disable a Rule

In `.markdownlint.json`:

```json
{
  "MD013": false
}
```

### Configure Rule Parameters

```json
{
  "MD013": {
    "line_length": 120,
    "code_blocks": false,
    "tables": false
  }
}
```

### Disable Inline Within a File

```markdown
<!-- markdownlint-disable MD013 -->
This text can be as long as needed without triggering an error.
<!-- markdownlint-restore -->
```

For a single line:

```markdown
<!-- markdownlint-disable-next-line MD041 -->
This is not a heading but it is the first line.
```

### Disable by Alias

```markdown
<!-- markdownlint-disable line-length -->
Long text...
<!-- markdownlint-restore -->
```

## Recommended Configurations

### Technical Documentation

```json
{
  "default": true,
  "MD013": false,
  "MD033": {
    "allowed_elements": ["br", "details", "summary", "sup", "sub", "img"]
  },
  "MD040": true,
  "MD045": true
}
```

### Project README

```json
{
  "default": true,
  "MD013": false,
  "MD033": {
    "allowed_elements": ["br", "p", "img", "a", "h1", "div"]
  },
  "MD041": true
}
```

### Blog Posts / Articles

```json
{
  "default": true,
  "MD013": { "line_length": 120 },
  "MD025": { "front_matter_title": "^\\s*title\\s*[:=]" },
  "MD040": true,
  "MD045": true
}
```
