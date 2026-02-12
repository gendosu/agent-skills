---
paths:
  - "**/*"
---

# Check Broken Links

## Rule

Before committing changes or creating pull requests, verify that all file references and links in the project are valid and point to existing files.

## Guidelines

- **Before git operations**: Check for broken links before commits, pull requests, and merges
- **File references**: Validate all file path references in documentation and code
- **Markdown links**: Check internal links in markdown files (e.g., `[text](./path/to/file.md)`)
- **Import statements**: Verify that imported files exist
- **Warning display**: Display clear warnings when broken links are detected, including:
  - Source file location
  - Broken link/reference
  - Expected file path
  - Suggestion for resolution

## Examples

### ❌ Broken Links to Report

```markdown
<!-- In README.md -->
See [documentation](./docs/missing-file.md) for details.

<!-- In skill.md -->
For more information, refer to [guide](../non-existent/guide.md).
```

```javascript
// In code
import config from './config/missing.json';
```

### ✅ Valid Links

```markdown
<!-- Properly referenced files -->
See [documentation](./docs/existing-file.md) for details.
```

```javascript
// Valid import
import config from './config/settings.json';
```

## Warning Format

When broken links are detected, display warnings in this format:

```
⚠️ WARNING: Broken link detected
  File: ./README.md:15
  Link: ./docs/missing-file.md
  Status: File not found
  Suggestion: Check if the file exists or update the link
```

## Rationale

- **Reliability**: Ensures documentation and code references are always valid
- **User experience**: Prevents users from following broken links
- **Maintenance**: Catches issues early before they reach production
- **Quality**: Maintains high documentation and code quality standards

## Scope

This rule applies to:
- Markdown files (`.md`)
- Documentation files
- Code files with import/require statements
- Configuration files with file references
- Skill definition files
