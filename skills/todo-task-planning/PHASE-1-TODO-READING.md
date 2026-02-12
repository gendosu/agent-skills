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

#### Sub-step 2.1: Parse `--pr` Flag

**Step 1: Split Arguments**
1. Split `$ARGUMENTS` on whitespace to get an array of tokens
2. First element is the file path (e.g., `TODO.md`, `docs/todos/feature-x.md`)
3. Remaining elements are command-line flags (e.g., `--pr`, `--branch`, `feature/auth`)

**Step 2: Detect `--pr` Flag**
1. Check if `$ARGUMENTS` contains the exact string `--pr`:
   - Search for `--pr` in the arguments list
   - If found: Set `HAS_PR_OPTION = true`
   - If not found: Set `HAS_PR_OPTION = false`

2. Log the result:
   ```
   Parsed --pr flag: HAS_PR_OPTION = [true/false]
   ```

#### Sub-step 2.2: Parse `--branch` Flag and Value

**Step 1: Detect `--branch` Flag**
1. Check if `$ARGUMENTS` contains the string `--branch`:
   - If NOT found:
     - Set `HAS_BRANCH_OPTION = false`
     - Set `BRANCH_NAME = ""` (empty string)
     - Set `IS_AUTO_GENERATED = false`
     - Skip to Sub-step 2.3

   - If found:
     - Set `HAS_BRANCH_OPTION = true`
     - Continue to Step 2 to extract the branch name value

**Step 2: Extract Branch Name Value**
1. Locate the position of `--branch` in `$ARGUMENTS`
2. Check the next token after `--branch`:
   - If next token exists AND does not start with `--`:
     - Set `BRANCH_NAME = [next token value]`
     - Set `IS_AUTO_GENERATED = false`
     - Example: `--branch feature/auth` → `BRANCH_NAME = "feature/auth"`

   - If next token does not exist OR starts with `--`:
     - Set `BRANCH_NAME = ""` (empty string, will be generated in Step 3)
     - Set `IS_AUTO_GENERATED = true`
     - Example: `--branch --pr` → `BRANCH_NAME = ""`, `IS_AUTO_GENERATED = true`

3. Log the result:
   ```
   Parsed --branch flag:
     HAS_BRANCH_OPTION = true
     BRANCH_NAME = "[value or empty]"
     IS_AUTO_GENERATED = [true/false]
   ```

#### Sub-step 2.3: Validation - PR Requires Branch

**Step 1: Apply Validation Rule**
1. Check the validation condition:
   - If `HAS_PR_OPTION = true` AND `HAS_BRANCH_OPTION = false`:
     - Automatically set `HAS_BRANCH_OPTION = true`
     - Set `IS_AUTO_GENERATED = true`
     - Set `BRANCH_NAME = ""` (will be auto-generated in Step 3)
     - Rationale: Pull requests require a branch, so enable branch creation automatically
     - Log: "PR flag detected without branch flag - automatically enabling branch creation"

2. Log final validation result:
   ```
   Validation complete:
     HAS_PR_OPTION = [value]
     HAS_BRANCH_OPTION = [value]
     IS_AUTO_GENERATED = [value]
   ```

#### Sub-step 2.4: Variable Summary Output

Output the final parsed values to ensure they are part of the conversation context:

```
=== Phase 1 Variables Summary ===
HAS_BRANCH_OPTION = [value]
HAS_PR_OPTION = [value]
BRANCH_NAME = [value]
IS_AUTO_GENERATED = [value]
=================================
```

**Variable Reference Table**:

| Variable | Type | Purpose | Set By |
|----------|------|---------|--------|
| `HAS_BRANCH_OPTION` | boolean | Whether branch creation is needed | `--branch` flag or PR validation |
| `HAS_PR_OPTION` | boolean | Whether PR creation is needed | `--pr` flag |
| `BRANCH_NAME` | string | Branch name (empty if auto-generated) | Explicit `--branch <name>` argument |
| `IS_AUTO_GENERATED` | boolean | Whether branch name needs generation | Flag without value or PR validation |

### 2.5: Variable Persistence (Critical Step)

After parsing all arguments and generating branch name (if needed in Step 3), explicitly persist the variables for use in later phases, especially Phase 9.

**Implementation:**

1. Create a summary block that lists all variables in a consistent format:
   ```
   === Phase 1 Variables Summary ===
   HAS_BRANCH_OPTION = [value]
   HAS_PR_OPTION = [value]
   BRANCH_NAME = [value]
   IS_AUTO_GENERATED = [value]
   ================================
   ```

2. Include this summary in your response to ensure it's part of the conversation context

3. These variables will be available to Phase 9 through the conversation history

**When to Execute:**
- After completing Step 2 (argument parsing)
- After completing Step 3 (branch name generation, if applicable)
- Before transitioning to Phase 2

**Purpose:**
- Ensure variables are explicitly recorded in the conversation
- Enable Phase 9 to retrieve these values through conversation history search
- Provide a consistent format for variable extraction

---

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
- BRANCH_NAME = "" (empty, will be generated in Step 3)
- IS_AUTO_GENERATED = true
```

**Example 3: PR flag auto-enables branch creation**
```
Input: /todo-task-planning TODO.md --pr
Result:
- HAS_PR_OPTION = true
- HAS_BRANCH_OPTION = true (auto-enabled by validation)
- BRANCH_NAME = "" (empty, will be generated in Step 3)
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

**Example 5: Branch flag at end of arguments**
```
Input: /todo-task-planning TODO.md --branch
Result:
- HAS_PR_OPTION = false
- HAS_BRANCH_OPTION = true
- BRANCH_NAME = "" (empty, will be generated in Step 3)
- IS_AUTO_GENERATED = true
```

### 3. Branch Name Auto-Generation (If Required)

**Trigger Condition:**
- Only execute this step if `IS_AUTO_GENERATED = true`
- If `IS_AUTO_GENERATED = false`, skip this step entirely and proceed to Step 4

#### Step 3.1: Read TODO File Title

**Step 1: Extract Title**
1. Read the first few lines of the TODO file (already read in Step 1)
2. Look for the first `#` heading (typically `# [Title]`)
3. Extract the title text (remove the `#` prefix and trim whitespace)

**Step 2: Handle Missing Title**
1. If no title found (no `#` heading in first 10 lines):
   - Use fallback: "task-implementation"
   - Log warning: "No title found in TODO file, using fallback"
   - Set `title = "task-implementation"`
2. If title found:
   - Set `title = [extracted title text]`
   - Log: "Extracted title: [title]"

#### Step 3.2: Determine Branch Type

**Step 1: Analyze Title for Keywords**
1. Convert title to lowercase for case-insensitive matching
2. Search for keywords in the following priority order:
   - If contains "bug", "fix", "error", "issue", "defect": Type = "bugfix"
   - If contains "feature", "add", "new", "implement", "create": Type = "feature"
   - If contains "refactor", "improve", "optimize", "restructure": Type = "refactor"
   - If contains "test", "testing", "spec": Type = "test"
   - If contains "doc", "documentation", "readme": Type = "docs"
   - If contains "chore", "update", "upgrade", "dependency": Type = "chore"
   - Default (no keyword match): Type = "feature"

**Step 2: Log Determination**
```
Branch type determined: [type]
Reason: [keyword found or "default"]
```

#### Step 3.3: Generate Branch Name

**Step 1: Sanitize Title**
1. Take the extracted title text
2. Apply the following transformations in order:
   - Convert to lowercase
   - Replace spaces with hyphens (`-`)
   - Remove all special characters except hyphens (keep only: a-z, 0-9, -)
   - Replace multiple consecutive hyphens with single hyphen (use iterative replacement until no consecutive hyphens remain)
   - Remove leading and trailing hyphens
   - Trim to maximum 50 characters (if longer, cut at last hyphen before character 50)
3. Handle edge cases:
   - If result is empty after sanitization (e.g., Japanese characters only), use fallback: "task-implementation"
   - Log warning if fallback is used: "Sanitization resulted in empty string, using fallback"

**Step 2: Construct Branch Name**
1. Combine type and sanitized title: `[type]/[sanitized-title]`
   - Example: `feature/user-authentication`
   - Example: `bugfix/login-error-handling`

**Step 3: Validate Git Naming Conventions**
1. Check the generated branch name against Git rules:
   - No consecutive hyphens (should already be handled in sanitization)
   - Does not start or end with hyphen (should already be handled)
   - No uppercase letters (should already be handled)
   - Contains only valid characters: a-z, 0-9, -, /

2. If validation fails:
   - Log error: "Generated branch name failed validation: [branch name]"
   - Use fallback: `feature/task-implementation`

**Step 4: Log Generated Name**
```
Generated branch name: [BRANCH_NAME]
Sanitization steps applied: lowercase, hyphen-separation, special char removal
```

#### Step 3.4: Store Branch Name

**Step 1: Update Variables**
1. Set the variable: `BRANCH_NAME = [generated branch name value]`
2. Confirm: `IS_AUTO_GENERATED = true` (should already be true from Step 2)

**Step 2: Include in Variable Summary**
1. This updated `BRANCH_NAME` will be included in the Phase 1 Variables Summary (Step 2.5)
2. Ensure the generated name is logged clearly for debugging

**Step 3: Log Completion**
```
✅ Branch name generation complete
  BRANCH_NAME = [value]
  IS_AUTO_GENERATED = true
```

#### Branch Name Generation Examples

**Example 1: Feature with clear title**
```
Title: "# User Authentication Implementation"
→ Type: feature (keyword: "implement")
→ Sanitized: "user-authentication-implementation"
→ Result: BRANCH_NAME = "feature/user-authentication-implementation"
```

**Example 2: Bug fix**
```
Title: "# Fix login error on mobile devices"
→ Type: bugfix (keyword: "fix")
→ Sanitized: "login-error-on-mobile-devices"
→ Result: BRANCH_NAME = "bugfix/login-error-on-mobile-devices"
```

**Example 3: Refactoring**
```
Title: "# Refactor database query optimization"
→ Type: refactor (keyword: "refactor")
→ Sanitized: "database-query-optimization"
→ Result: BRANCH_NAME = "refactor/database-query-optimization"
```

**Example 4: Title with special characters**
```
Title: "# Add new feature: Real-time notifications!"
→ Type: feature (keyword: "add")
→ Sanitized: "add-new-feature-real-time-notifications"
→ Result: BRANCH_NAME = "feature/add-new-feature-real-time-notifications"
```

**Example 5: No title found**
```
Title: (not found)
→ Type: feature (default)
→ Fallback title: "task-implementation"
→ Result: BRANCH_NAME = "feature/task-implementation"
```

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

## Post-Implementation Verification Checklist

After modifying Phase 1 implementation, verify the following:

- [ ] **Argument Parsing Verification (Step 2)**
  - [ ] `--pr` flag detection works correctly (Sub-step 2.1)
  - [ ] `--branch` flag detection works correctly (Sub-step 2.2)
  - [ ] Branch name value extraction works correctly
  - [ ] `IS_AUTO_GENERATED` flag is set appropriately
  - [ ] PR requires branch validation logic works (Sub-step 2.3)
  - [ ] Variable Summary output is generated (Sub-step 2.4)

- [ ] **Variable Persistence Verification (Step 2.5)**
  - [ ] "Phase 1 Variables Summary" block is output to conversation
  - [ ] All 4 variables are included in summary (HAS_BRANCH_OPTION, HAS_PR_OPTION, BRANCH_NAME, IS_AUTO_GENERATED)
  - [ ] Summary format is consistent and parseable by Phase 9
  - [ ] Variables are accessible in later phases (verify in Phase 9)

- [ ] **Branch Name Generation Verification (Step 3)**
  - [ ] Step 3 only executes when `IS_AUTO_GENERATED = true`
  - [ ] Title extraction works (Step 3.1)
  - [ ] Branch type determination works (Step 3.2)
  - [ ] Sanitization logic correctly handles edge cases:
    - [ ] Japanese characters (should fallback)
    - [ ] Special characters (should be removed)
    - [ ] Multiple consecutive spaces (should become single hyphen)
    - [ ] Uppercase letters (should be lowercased)
    - [ ] 50+ character titles (should be truncated at last hyphen)
  - [ ] Git naming convention validation works (Step 3.3)
  - [ ] Fallback to "feature/task-implementation" works when needed
  - [ ] Generated branch name is stored in `BRANCH_NAME` variable

- [ ] **End-to-End Testing**
  - [ ] Test Case 1: `--branch` only (no value) → auto-generation triggered
  - [ ] Test Case 2: `--branch feature/test` → user-provided name used
  - [ ] Test Case 3: `--pr` only → auto branch creation + auto-generation
  - [ ] Test Case 4: No flags → all variables false/empty
  - [ ] Test Case 5: `--pr --branch custom-name` → both flags + custom name

**Reference**: See `/Users/takahashi.g/products/gendosu/agent-skills/docs/memory/debugging/test-6-phase1-unit.log` for unit test results.

---

[← Previous: Phase 0](PHASE-0-KEY-GUIDELINES.md) | [Next: Phase 2 →](PHASE-2-EXPLORE.md)
