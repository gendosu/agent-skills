---
paths:
  - "**/*"
---

# Use Relative Paths

## Rule

When referencing files within the project, always use relative paths instead of absolute paths.

## Guidelines

- **File references**: Use relative paths (e.g., `./docs/README.md` or `../../config/settings.json`) instead of absolute paths (e.g., `/Users/username/project/docs/README.md`)
- **Documentation**: When documenting file locations, prefer relative paths from a clear context point
- **Code**: In code files, use relative imports and paths when referencing project files
- **Configuration**: Configuration files should use relative paths for project-internal resources

## Examples

### ❌ Bad (Absolute Path)
```
/Users/username/project/skills/commit/skill.md
```

### ✅ Good (Relative Path)
```
./skills/commit/skill.md
../../skills/commit/skill.md
skills/commit/skill.md
```

## Rationale

- **Portability**: Relative paths work across different environments and users
- **Maintainability**: Easier to reorganize project structure
- **Security**: Avoids exposing local file system structure
