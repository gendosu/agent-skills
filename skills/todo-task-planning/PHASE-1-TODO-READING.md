# Phase 1: TODO File Reading

[← Previous: Phase 0](PHASE-0-KEY-GUIDELINES.md) | [Next: Phase 2 →](PHASE-2-EXPLORE.md)

---

## Overview

Phase 1 reads and parses the TODO file specified in `$ARGUMENTS`, extracts context, and prepares variables for subsequent phases.

**Critical Output:**
- `HAS_PR_OPTION`: Boolean indicating if PR creation is requested
- `HAS_BRANCH_OPTION`: Boolean indicating if branch creation is requested
- `BRANCH_NAME`: String containing branch name (empty if auto-generated)
- `IS_AUTO_GENERATED`: Boolean indicating if branch name needs generation

---

## Execution Steps

### 1. Reading $ARGUMENTS File

- Read the specified file with Read tool
- Extract information on tasks, requirements, and tech stack
- Determine exploration thoroughness (quick/medium/very thorough)

### 2. Git Workflow Options Preparation

Parse the `$ARGUMENTS` string to extract file path and command-line flags, then prepare variables for subsequent phase execution.

#### Argument Parsing Procedure

**a) Parse `$ARGUMENTS` string**:
- Split `$ARGUMENTS` on whitespace to get an array of tokens
- First element is the file path (e.g., `TODO.md`, `docs/todos/feature-x.md`)
- Remaining elements are command-line flags (e.g., `--pr`, `--branch`, `feature/auth`)

**b) Detect `--pr` flag**:
```
IF `$ARGUMENTS` contains `--pr` THEN
    → Set `HAS_PR_OPTION = true`
ELSE
    → Set `HAS_PR_OPTION = false`
END IF
```

**c) Detect `--branch` flag and extract value**:
```
IF `$ARGUMENTS` contains `--branch` THEN
    → Set `HAS_BRANCH_OPTION = true`
    → Find the token immediately after `--branch`
    IF next token exists AND does NOT start with `--` THEN
        → Set `BRANCH_NAME = [token value]`
        → Set `IS_AUTO_GENERATED = false`
    ELSE
        → Set `BRANCH_NAME = ""` (empty string, will be generated in next step)
        → Set `IS_AUTO_GENERATED = true`
    END IF
ELSE
    → Set `HAS_BRANCH_OPTION = false`
    → Set `BRANCH_NAME = ""` (empty string)
    → Set `IS_AUTO_GENERATED = false`
END IF
```

**d) Validation logic - PR requires branch**:
```
IF `HAS_PR_OPTION = true` AND `HAS_BRANCH_OPTION = false` THEN
    → Automatically set `HAS_BRANCH_OPTION = true`
    → Set `IS_AUTO_GENERATED = true`
    → Set `BRANCH_NAME = ""` (will be auto-generated in next step)
    → Rationale: Pull requests require a branch, so enable branch creation automatically
END IF
```

**e) Variable summary**:

| Variable | Type | Purpose | Set By |
|----------|------|---------|--------|
| `HAS_BRANCH_OPTION` | boolean | Whether branch creation is needed | `--branch` flag or PR validation |
| `HAS_PR_OPTION` | boolean | Whether PR creation is needed | `--pr` flag |
| `BRANCH_NAME` | string | Branch name (empty if auto-generated) | Explicit `--branch <name>` argument |
| `IS_AUTO_GENERATED` | boolean | Whether branch name needs generation | Flag without value or PR validation |

#### Parsing Examples

**Example 1: Both flags with explicit branch name**
```
Input: /todo-task-planning TODO.md --pr --branch feature/auth
Result:
- HAS_PR_OPTION = true
- HAS_BRANCH_OPTION = true
- BRANCH_NAME = "feature/auth"
- IS_AUTO_GENERATED = false
```

**Example 2: Branch flag without value**
```
Input: /todo-task-planning docs/todos/feature-x.md --branch
Result:
- HAS_PR_OPTION = false
- HAS_BRANCH_OPTION = true
- BRANCH_NAME = "" (empty, will be generated)
- IS_AUTO_GENERATED = true
```

**Example 3: PR flag auto-enables branch creation**
```
Input: /todo-task-planning TODO.md --pr
Result:
- HAS_PR_OPTION = true
- HAS_BRANCH_OPTION = true (auto-enabled by validation)
- BRANCH_NAME = "" (empty, will be generated)
- IS_AUTO_GENERATED = true (set by validation)
```

**Example 4: No flags**
```
Input: /todo-task-planning docs/feature.md
Result:
- HAS_PR_OPTION = false
- HAS_BRANCH_OPTION = false
- BRANCH_NAME = "" (empty)
- IS_AUTO_GENERATED = false
```

### 3. Branch Name Generation (if --branch option specified without value)

- Read TODO file title and overview
- Determine branch type:
  - `feature/` - New functionality (default for most implementations)
  - `bugfix/` - Bug fixes mentioned in TODO
  - `refactor/` - Code restructuring mentioned
  - `chore/` - Tooling, dependencies, environment setup
  - `docs/` - Documentation-focused tasks
- Extract key feature name (2-4 words)
- Convert to lowercase, hyphen-separated English
- Validate against Git naming conventions
- Set generated branch name to `BRANCH_NAME` variable and set `IS_AUTO_GENERATED = true`
- Example: "ActionLog通知実装" → `feature/actionlog-notification`

### 4. Context Preparation for Exploration

- Identify feature areas and related keywords
- Determine exploration scope (file patterns, directories)
- Check existing docs/memory research results (to avoid duplicate research)

---

## Variable Persistence

**IMPORTANT**: Variables set in Phase 1 persist throughout all subsequent phases (Phase 2-10).

**Phase 1 Variables Used in Later Phases**:
- `HAS_PR_OPTION`, `HAS_BRANCH_OPTION`, `BRANCH_NAME`, `IS_AUTO_GENERATED`
  - Set in: Phase 1 Step 2 (argument parsing)
  - Used in: Phase 9 (conditional task insertion)
  - Scope: Available throughout entire skill execution

**Variable Lifecycle**:
```
Phase 1 → Set variables
    ↓
Phase 9 → Use variables for conditional logic
```

---

[← Previous: Phase 0](PHASE-0-KEY-GUIDELINES.md) | [Next: Phase 2 →](PHASE-2-EXPLORE.md)
