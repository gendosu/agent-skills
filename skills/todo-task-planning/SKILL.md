---
name: todo-task-planning
description: Execute task planning based on the specified file and manage questions[/todo-task-planning file_path --pr --branch branch_name]
argument-hint: <file_path> [--pr] [--branch <name>]
arguments:
  - name: file_path
    description: Path to the file for task planning execution
    required: true
  - name: --pr
    description: Create a pull request after task completion (flag)
    required: false
  - name: --branch
    description: Branch name to create and use for task execution (optional value flag)
    required: false
user-invocable: true
---

**Command arguments**: $ARGUMENTS

## Usage

```
/todo-task-planning <file_path> [--pr] [--branch <name>]
```

### Arguments
- `file_path` (required): Path to the file for task planning execution
- `--pr` (optional): Create a pull request after task completion. When specified, tasks will include branch creation (auto-generated if --branch not specified), commits, and PR creation
- `--branch <name>` (optional): Branch name to create and use for task execution. Creates the specified branch and commits all changes to it. Can be used independently or with --pr option

## Command Overview

This command reads the specified file ($ARGUMENTS) and performs comprehensive task planning.
It can be executed repeatedly on the same file and also manages, confirms, and updates questions.

**Important**: This command is designed to be executed repeatedly.
Do not question the fact that the same command will be executed multiple times; analyze and plan with a fresh perspective each time.

**Important**: The file specified in $ARGUMENTS will be the TODO management file.
Avoid including specifications and research results in this file as much as possible; use docs/memory to store research results.
Also, do not excessively abbreviate research results; retain them.
Check each time whether you have researched something in the past.
Do not neglect checking to avoid duplicating research results and tasks.

## Branch and PR Options Usage

### Option Behavior

**`--branch [branch_name]` option:**
- **Adds a branch creation task at the beginning of the task list**
- If `branch_name` is provided: Uses the specified branch name
- If `branch_name` is omitted: Auto-generates branch name following Git naming conventions
- All commits during task execution will be made to this branch
- Can be used independently without `--pr`
- Useful when you want to work on a feature branch but don't need a PR yet

**Branch name auto-generation rules:**
- Analyzes TODO file content to determine branch type and purpose
- Follows Git naming conventions: `{type}/{descriptive-name}`
- Types: `feature/`, `bugfix/`, `refactor/`, `chore/`, `docs/`
- Format: lowercase, hyphen-separated, English
- Example: `feature/actionlog-email-notification`

**`--pr` option:**
- Includes all `--branch` functionality (branch creation and commits)
- **Adds a pull request creation task at the end of the task list**
- If `--branch` is not specified, a branch name will be auto-generated
- The PR will include all changes made during task execution
- If PR template instructions exist in CLAUDE.md or similar files, those templates will be used

### Usage Examples

```bash
# Example 1: Create branch with auto-generated name (no PR)
/todo-task-planning TODO.md --branch
# → Generates branch name like: feature/actionlog-notification

# Example 2: Create branch with specific name (no PR)
/todo-task-planning docs/todos/feature-x.md --branch feature/user-auth

# Example 3: Create PR with auto-generated branch name
/todo-task-planning docs/todos/feature-x.md --pr
# → Auto-generates branch name and creates PR

# Example 4: Create PR with specific branch name
/todo-task-planning docs/todos/feature-x.md --pr --branch feature/user-auth

# Example 5: Basic task planning (no branch, no PR)
/todo-task-planning docs/todos/feature-x.md
```

### Implementation Guidance

When these options are specified, the task planning should include:

**For `--branch` option:**
- **Branch Name Determination:**
  - If branch name is provided: Use as-is (validate against naming conventions)
  - If branch name is omitted: Auto-generate following this logic:
    1. Read TODO file title and content
    2. Determine branch type based on task nature:
       - `feature/` - New functionality implementation
       - `bugfix/` - Bug fixes, issue resolution
       - `refactor/` - Code restructuring without behavior change
       - `chore/` - Development environment, dependencies, tooling
       - `docs/` - Documentation updates
    3. Extract key feature/issue name from TODO (2-4 words max)
    4. Convert to lowercase, hyphen-separated English
    5. Format: `{type}/{descriptive-name}`
    6. Example: "ActionLog Email Notification" → `feature/actionlog-email-notification`
- Task to create the determined/generated branch at the beginning
- All modification tasks should indicate they will be committed to this branch
- No PR-related tasks

**For `--pr` option:**
- Task to create a branch (using specified name or auto-generated)
- All modification tasks with commit instructions
- Final task to create a pull request with proper description
- PR description should summarize all changes made

## Reference Documentation

- [todo-task-run skill](../todo-task-run/SKILL.md)
- [key-guidelines skill](../key-guidelines/SKILL.md)

## Core Guidelines

Before starting any task, read and follow `/key-guidelines`

## [CRITICAL]Important Implementation Requirements

**MANDATORY**: This command MUST update the $ARGUMENTS file (the file specified as a parameter)
- **Main Claude executor** (not a subagent) uses Edit or Write tool to update files
- After calling subagents in Phase 0, update the $ARGUMENTS file with those results in Phase 4
- Add new task planning results in a structured format while preserving existing content
- After file update is complete, confirm, verify, and report the updated content
- **CRITICAL**: The $ARGUMENTS file update is NOT optional - it must be executed in every run

## Processing Flow

### [CRITICAL]Phase Dependency and Execution Rules

**[PROHIBITED]PROHIBITED PHASE SHORTCUTS - MUST FOLLOW SEQUENTIAL FLOW ⛔**

The following phase shortcuts are **STRICTLY PROHIBITED**:
- [NG]**Phase 0 → Phase 4 direct transition** (MOST COMMON VIOLATION)
- [NG]**Phase 0 → Phase 5 direct transition**
- [NG]**Phase 1 → Phase 4 direct transition** (skipping Phase 2-3)
- [NG]**Phase 2 → Phase 4 direct transition** (skipping Phase 3)

**Why These Shortcuts Are Dangerous:**
- **Data Loss**: Phase 1 existing task analysis will be ignored
- **No Integration**: New tasks won't integrate with existing TODO.md content
- **Missing Validation**: User questions won't be asked, leading to incorrect assumptions
- **File Creation Failures**: docs/memory files won't be created at proper time

### 📊 Phase Dependency Flow Chart

```
┌─────────────────────────────────────────────────────────────────┐
│                    PHASE EXECUTION FLOW                         │
│                 (MUST FOLLOW SEQUENTIAL ORDER)                  │
└─────────────────────────────────────────────────────────────────┘

Phase 0: TODO File Reading → [詳細](PHASE-0-TODO-READING.md)          [OK]MANDATORY
└─ Read and parse $ARGUMENTS file
   └─ Parse --pr and --branch options
   └─ Output: HAS_PR_OPTION, HAS_BRANCH_OPTION, BRANCH_NAME
              ↓
Phase 1: Explore Subagent → [詳細](PHASE-1-EXPLORE.md)                [OK]MANDATORY
└─ Call Task tool (Explore)
   └─ Output: exploration_results
              ↓
Phase 2: Plan Subagent → [詳細](PHASE-2-PLAN.md)                      [OK]MANDATORY
└─ Call Task tool (Plan) with exploration_results
   └─ Output: planning_results
              ↓
Phase 3: project-manager skill → [詳細](PHASE-3-PROJECT-MANAGER.md)   [OK]MANDATORY
└─ Call Task tool (project-manager) with exploration + planning results
   └─ Output: strategic_plan
              ↓
Phase 4: Verification → [詳細](PHASE-4-VERIFICATION.md)               [OK]MANDATORY
└─ Verify all subagents completed successfully
   └─ Verify: exploration_results, planning_results, strategic_plan exist
              ↓
Phase 5: File Analysis → [詳細](PHASE-5-ANALYSIS.md)                  [OK]MANDATORY
└─ Read $ARGUMENTS file
   └─ Output: existingTasks, taskProgress
              ↓
Phase 6: Task Analysis & Breakdown → [詳細](PHASE-6-BREAKDOWN.md)     [OK]MANDATORY
└─ Input: Phase 0-4 results + Phase 5 results
   └─ Output: Task breakdown, feasibility analysis
              ↓
Phase 7: Question Management → [詳細](PHASE-7-QUESTIONS.md)           [WARNING]CONDITIONAL
├─ CONDITION A: Questions exist         [OK]MANDATORY
│  └─ Execute AskUserQuestion tool
│  └─ Wait for user responses
│  └─ Create questions.md file
│  └─ Output: User decisions recorded
├─ CONDITION B: No questions            [OK]ALLOWED (Must document reason)
│  └─ Proceed to Phase 8
│  └─ Document why no questions needed
└─ [PROHIBITED]GATE: Phase 8 entrance checkpoint
              ↓
Phase 8: File Update → [詳細](PHASE-8-UPDATE.md)                      [OK]MANDATORY
├─ Create docs/memory files (exploration, planning, questions)
├─ Update $ARGUMENTS file with task checklist
└─ Insert branch/PR tasks if needed
              ↓
Phase 9: Verification & Feedback → [詳細](PHASE-9-VERIFICATION.md)    [OK]MANDATORY
└─ Verify file updates, AskUserQuestion execution
   └─ Report to user

```

### 🔐 Phase Requirement Markers

| Phase | Status | Skippable? | Dependencies | Critical Output |
|-------|--------|------------|--------------|-----------------|
| **Phase 0** | [OK]MANDATORY | 🚫 NO | None | HAS_PR_OPTION, HAS_BRANCH_OPTION, BRANCH_NAME |
| **Phase 1** | [OK]MANDATORY | 🚫 NO | Phase 0 | exploration_results |
| **Phase 2** | [OK]MANDATORY | 🚫 NO | Phase 1 | planning_results |
| **Phase 3** | [OK]MANDATORY | 🚫 NO | Phase 2 | strategic_plan |
| **Phase 4** | [OK]MANDATORY | 🚫 NO | Phase 3 | Verification status |
| **Phase 5** | [OK]MANDATORY | 🚫 NO | Phase 4 | existingTasks, taskProgress |
| **Phase 6** | [OK]MANDATORY | 🚫 NO | Phase 0-5 | Task breakdown, feasibility |
| **Phase 7** | [WARNING]CONDITIONAL | 🚫 NO (See conditions) | Phase 6 | User decisions (if questions exist) |
| **Phase 8** | [OK]MANDATORY | 🚫 NO | Phase 0-7 | Updated $ARGUMENTS file, docs/memory files |
| **Phase 9** | [OK]MANDATORY | 🚫 NO | Phase 8 | Verification report |

**Phase 7 Conditions:**
- [OK]**Questions exist**: MUST execute AskUserQuestion tool and wait for responses
- [OK]**No questions**: MUST proceed to Phase 8 and document reason in Phase 9

### [WARNING]Critical Phase Transition Rules

**Rule 1: Sequential Execution Only**
- Each phase MUST complete before the next phase begins
- No parallel execution of phases
- No skipping of phases
- Each Phase is independent and must be executed in a separate turn/message

**Rule 2: Subagent Phases (1-3) Must Execute Sequentially**
```
[NG]WRONG FLOW (Parallel Execution):
Phase 1 (Explore) + Phase 2 (Plan) + Phase 3 (project-manager) → Called in same message
                    ↓
            PARALLEL EXECUTION
                    ↓
    Result: Phase 2/3 start before Phase 1 completes, missing dependencies

[OK]CORRECT FLOW (Sequential Execution):
Phase 0 → Phase 1 (wait) → Phase 2 (wait) → Phase 3 (wait) → Phase 4 → ...
   ↓         ↓                ↓                ↓                ↓
  TODO    Explore          Plan          project-mgr      Verify
 Reading  (output)       (needs P1)      (needs P1+P2)   (check all)
```

**Rule 3: Never Skip to Phase 8 (File Update)**
- Phase 8 requires outputs from ALL previous phases
- Skipping Phase 5-7 causes data loss and missing integration
- Phase 5 (File Analysis) results are mandatory for Phase 6
- Phase 7 (Questions) is the entrance gate to Phase 8

**Rule 4: Phase 7 is a Mandatory Checkpoint**
- Even if no questions exist, Phase 7 MUST be executed to document this fact
- Phase 8 entrance gate verifies Phase 7 completion

## Variable Scope and Persistence

**IMPORTANT**: Variables set in Phase 0 persist throughout all subsequent phases (Phase 1-5).

**Phase 0.1 Variables Used in Later Phases**:
- `HAS_PR_OPTION`, `HAS_BRANCH_OPTION`, `BRANCH_NAME`, `IS_AUTO_GENERATED`
  - Set in: Phase 0.1 Step 2 (argument parsing)
  - Used in: Phase 4.10 (conditional task insertion)
  - Scope: Available throughout entire skill execution

**Variable Lifecycle**:
```
Phase 0.1 → Set variables
    ↓
Phase 4 → Use variables for conditional logic
```

---

## Related Documentation

For detailed information about each phase, see:

- [Phase 0: TODO Reading](PHASE-0-TODO-READING.md) - Read and parse TODO file and arguments
- [Phase 1: Explore](PHASE-1-EXPLORE.md) - Explore subagent execution
- [Phase 2: Plan](PHASE-2-PLAN.md) - Plan subagent execution
- [Phase 3: Project Manager](PHASE-3-PROJECT-MANAGER.md) - project-manager skill execution
- [Phase 4: Verification](PHASE-4-VERIFICATION.md) - Verify all subagents completed
- [Phase 5: Analysis](PHASE-5-ANALYSIS.md) - File Analysis and Status Confirmation
- [Phase 6: Breakdown](PHASE-6-BREAKDOWN.md) - Task Analysis and Breakdown
- [Phase 7: Questions](PHASE-7-QUESTIONS.md) - Question Management and User Confirmation
- [Phase 8: Update](PHASE-8-UPDATE.md) - $ARGUMENTS File Update and Branch/PR Creation
- [Phase 9: Verification](PHASE-9-VERIFICATION.md) - Verification and Feedback

Additional resources:

- [Advanced Usage](ADVANCED-USAGE.md) - Iterative Execution, Best Practices, Common Mistakes
- [Examples](EXAMPLES.md) - Usage Examples and Output Formats
