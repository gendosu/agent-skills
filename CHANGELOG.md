# Changelog

All notable changes to the CCCP plugin will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [3.4.5] - 2026-02-12

### Changed

- **Documentation**: Simplified plugin description and README structure for improved clarity
  - Updated plugin.json description: removed redundant "agent" terminology for consistency
  - Simplified README.md table structure by removing "Type" column from features table
  - Reorganized skills section: removed subsection headers, unified heading levels (#### → ###)
  - Consolidated usage examples into single comprehensive code block
  - Applied identical structural improvements to README.ja.md for English-Japanese consistency

## [3.4.4] - 2026-02-12

### Fixed

- **todo-task-planning**: Fixed `--branch` flag functionality to properly insert branch creation tasks
  - Resolved 8 implementation gaps between specification and executable code
  - Gap 1: Implemented variable persistence mechanism (Phase 1 → Phase 9)
  - Gap 2: Converted Phase 1 Step 2 argument parsing from pseudocode to executable instructions
  - Gap 3: Converted Phase 1 Step 3 branch name generation from pseudocode to executable instructions
  - Gap 4: Converted Phase 9 branch task insertion from pseudocode to executable instructions
  - Gap 5: Converted Phase 9 PR task insertion from pseudocode to executable instructions
  - Gap 6: Added error handling and fallback logic for edge cases
  - Gap 7: Enhanced verification and logging steps throughout execution
  - Gap 8: Implemented deduplication logic for Phase 0 task replacement

### Added

- **todo-task-planning**: Added comprehensive edge case handling documentation
  - New EDGE-CASE-HANDLING.md file documenting 20 edge cases and recovery strategies
  - Phase 1 argument parsing edge cases (7 cases)
  - Phase 1 branch name generation edge cases (7 cases)
  - Phase 9 variable retrieval edge cases (2 cases)
  - Phase 9 task insertion edge cases (4 cases)

### Changed

- **todo-task-planning**: Enhanced PHASE-1-TODO-READING.md with executable code
  - Added Step 2.5 for variable persistence (Phase 1 Variables Summary output)
  - Converted Step 2 pseudocode to 4 executable sub-steps (2.1-2.4)
  - Converted Step 3 pseudocode to 4 executable sub-steps (3.1-3.4)
  - Added Post-Implementation Verification Checklist section

- **todo-task-planning**: Enhanced PHASE-9-UPDATE.md with executable code
  - Added Step 9.5 for variable retrieval from Phase 1
  - Converted Sub-step 10.1 (branch task insertion) to 6 executable steps
  - Converted Sub-step 10.2 (PR task insertion) to 6 executable steps
  - Implemented INSERT/REPLACE/APPEND strategies for task insertion
  - Added Post-Implementation Verification Checklist section

- **todo-task-planning**: Enhanced VERIFICATION-PROTOCOLS.md
  - Added Phase 1 Argument Parsing Verification Protocol
  - Added Phase 9 Conditional Task Insertion Verification Protocol
  - Documented expected results for 5 flag combination patterns
  - Included recovery procedures for verification failures

### Testing

- All 23 test cases passed (100% success rate)
  - Phase 1 argument parsing: 7/7 tests passed
  - Phase 9 integration testing: 4/4 scenarios passed
  - Edge case testing: 5/5 cases passed
  - Phase 1 unit testing: 7/7 tests passed
- Verified all 4 flag combinations work correctly:
  - No flags: no tasks inserted
  - `--branch` only: branch task inserted
  - `--pr` only: both tasks inserted (auto-enables --branch)
  - `--pr --branch`: both tasks inserted

## [3.4.3] - 2026-02-11

### Fixed

- **todo-task-planning**: Fixed incorrect tool usage for project-manager in Phase 4
  - Changed from Task tool (subagent_type: "project-manager") to Skill tool (skill: "project-manager")
  - Updated PHASE-4-PROJECT-MANAGER.md to use Skill tool instead of Task tool
  - Updated ADVANCED-USAGE.md code examples to reflect correct Skill tool usage
  - Updated SKILL.md phase flow diagram to indicate Skill tool usage
  - project-manager is a skill, not a subagent, and must be invoked via Skill tool

## [3.4.2] - 2026-02-10

### Removed

- **Configuration**: Removed `.claude/skills` symbolic link file
  - Cleanup of legacy symbolic link no longer needed
  - Project uses root-level `skills/` directory structure

## [3.4.1] - 2026-02-10

### Changed

- **todo-task-planning**: Unified conditional logic language to English in PHASE-9-UPDATE.md
  - Translated Branch Creation Task (Conditional) section to English
  - Translated PR Creation Tasks (Conditional) section to English
  - Translated Conditional Behavior Summary section to English
  - Updated SKILL.md Phase 9 description to reflect conditional task insertion logic
  - Standardized Decision Point, Variables Referenced, and Insertion Rules sections
  - Improved consistency with overall documentation structure

## [3.4.0] - 2026-02-10

### Added

- **todo-task-planning**: Added Phase 0 to read and apply key-guidelines skill at the start of execution
  - Ensures development principles (DRY, KISS, YAGNI, SOLID, TDD, Micro-commits) are applied throughout task planning
  - New PHASE-0-KEY-GUIDELINES.md file with Skill tool invocation for key-guidelines
  - Outputs `guidelines_loaded` boolean to confirm successful loading

### Changed

- **todo-task-planning**: Renumbered existing phases from 0-9 to 1-10 to accommodate new Phase 0
  - Phase 0 (TODO Reading) → Phase 1
  - Phase 1 (Explore) → Phase 2
  - Phase 2 (Plan) → Phase 3
  - Phase 3 (Project Manager) → Phase 4
  - Phase 4 (Verification) → Phase 5
  - Phase 5 (Analysis) → Phase 6
  - Phase 6 (Breakdown) → Phase 7
  - Phase 7 (Questions) → Phase 8
  - Phase 8 (Update) → Phase 9
  - Phase 9 (Verification) → Phase 10
  - Updated all phase references, navigation links, flowcharts, and documentation in SKILL.md
  - Updated prohibited phase shortcuts to reflect new numbering
  - Updated variable scope documentation to reference Phase 1 instead of Phase 0

## [3.3.0] - 2026-02-10

### Fixed

- **todo-task-run**: Fixed critical issue where session would terminate prematurely despite incomplete tasks remaining
  - Added explicit continuation check after each task completion
  - Created dedicated "Continuation Check Procedure" section with clear visual indicators
  - Enhanced "Final Completion Process" with explicit prerequisite verification
  - Prerequisites must be satisfied before final completion: (1) ALL tasks marked complete, (2) ALL blockers resolved
  - Added runtime checkpoints to verify incomplete task presence before proceeding to completion

### Added

- **todo-task-run**: Added visual execution flow diagram illustrating task execution loop
  - Shows decision flow: Task Selection → Execution → Continuation Check → Next Task / Final Completion
  - Clarifies when to continue vs when to complete the session
  - Located in Task Execution Template section for immediate reference

- **todo-task-run**: Added execution checkpoints for better process control
  - Checkpoint after each task: Re-read TODO.md and verify incomplete task count
  - Checkpoint before final completion: Verify all prerequisite conditions met
  - Prevents premature session termination with clear verification steps

### Changed

- **todo-task-run**: Restructured task execution documentation for improved clarity
  - Separated "Continuation Check Procedure" from "Final Completion Process" into distinct sections
  - Moved continuation logic before completion logic to emphasize continuation priority
  - Added clear visual markers ([CONTINUE] vs [COMPLETE]) for quick identification
  - Improved section hierarchy: Task Execution Template → Continuation Check → Final Completion

- **todo-task-run**: Reduced documentation redundancy by 14.5% (905 lines → 774 lines)
  - Consolidated duplicate continuation check instructions into single authoritative section
  - Removed redundant TypeScript pseudocode examples while preserving critical logic
  - Merged overlapping CRITICAL warnings into cohesive verification procedures
  - Enhanced cross-referencing between related sections for better maintainability

## [3.2.0] - 2026-02-10

### Changed

- **Structure**: Flatten Phase structure in todo-task-planning skill to prevent parallel execution
  - Removed sub-phase structure (Phase 0.1-0.5) that was causing parallel execution
  - Split Phase 0 into 5 independent Phases (0-4): TODO Reading, Explore, Plan, project-manager, Verification
  - Renumbered existing Phases (1-5 → 5-9): Analysis, Breakdown, Questions, Update, Verification
  - Each Phase is now independent and must be executed in a separate turn/message
  - Updated all Phase references and navigation links across all documentation files
  - **Breaking Change**: Phase numbering has changed (Phase 1 is now Phase 5, etc.)
  - **Benefit**: Prevents Explore/Plan/project-manager from being called in parallel
  - **Result**: Sequential execution is now enforced by Phase structure, not just documentation

### Removed

- `PHASE-0-PREPARATION.md` - Split into 5 separate Phase files

### Added

- `PHASE-0-TODO-READING.md` - Phase 0: TODO File Reading
- `PHASE-1-EXPLORE.md` - Phase 1: Explore Subagent
- `PHASE-2-PLAN.md` - Phase 2: Plan Subagent
- `PHASE-3-PROJECT-MANAGER.md` - Phase 3: project-manager skill
- `PHASE-4-VERIFICATION.md` - Phase 4: Verification

### Renamed

- `PHASE-1-ANALYSIS.md` → `PHASE-5-ANALYSIS.md`
- `PHASE-2-BREAKDOWN.md` → `PHASE-6-BREAKDOWN.md`
- `PHASE-3-QUESTIONS.md` → `PHASE-7-QUESTIONS.md`
- `PHASE-4-UPDATE.md` → `PHASE-8-UPDATE.md`
- `PHASE-5-VERIFICATION.md` → `PHASE-9-VERIFICATION.md`

## [3.1.8] - 2026-02-10

### Changed

- **Documentation**: Clarify sequential execution requirements in PHASE-0-PREPARATION.md
  - Removed redundant "IN PARALLEL" emphasis that conflicted with sequential execution requirement
  - Removed outdated statement about parallel execution causing data dependency failures
  - Simplifies the explanation to focus on actual execution behavior and dependencies

## [3.1.7] - 2026-02-09

### Changed

- **Documentation**: Clarify sequential execution requirements in ADVANCED-USAGE.md workflow example
  - Added explicit completion conditions to Phase 0.3 and Phase 0.4
  - Phase 0.3 now shows "(after Explore completes)" to indicate dependency on Phase 0.2
  - Phase 0.4 now shows "(after Plan completes)" to indicate dependency on Phase 0.3
  - Reinforces the sequential execution requirement established in version 3.1.5
  - Prevents misinterpretation of Phase 0 workflow as parallel execution

## [3.1.6] - 2026-02-09

### Changed

- **Documentation**: Replace emojis with text-based symbols in todo-task-planning
  - Replaced emojis (❌, ✅, ⚠️, 🚨, ⛔) with text symbols ([NG], [OK], [WARNING], [CRITICAL], [PROHIBITED])
  - Updated ADVANCED-USAGE.md, PHASE-0-PREPARATION.md, and SKILL.md
  - Improves clarity and prevents misinterpretation of sequential execution requirements
  - Addresses issue where emoji-based notation was causing parallel execution to be suggested

## [3.1.5] - 2026-02-09

### Changed

- **Documentation**: Enforce sequential execution of Phase 0 subagents in todo-task-planning
  - Added CRITICAL EXECUTION RULE section prohibiting multiple Task tool calls in single message
  - Added execution requirements to Phase 0.2 (Explore), 0.3 (Plan), and 0.4 (project-manager)
  - Strengthened checkpoints with mandatory verification steps between phases
  - Added concrete examples of correct vs incorrect execution patterns
  - Clarified that only ONE Task tool should be called per message to prevent parallel execution
  - Prevents data dependency failures when Plan requires exploration_results and project-manager requires both results

## [3.1.4] - 2026-02-09

### Changed

- **Documentation**: Extracted verification protocols from todo-task-planning into dedicated document
  - Created VERIFICATION-PROTOCOLS.md (454 lines) consolidating all verification logic
  - Reduced PHASE-4-UPDATE.md from 402 to 146 lines (63.7% reduction)
  - Reduced PHASE-5-VERIFICATION.md from 123 to 57 lines (53.7% reduction)
  - Improved maintainability with centralized verification protocols
  - Enhanced modularity with single responsibility principle
  - Simplified phase files while preserving all verification functionality

## [3.1.3] - 2026-02-08

### Changed

- **Documentation**: Split todo-task-planning/SKILL.md into phase-based files for improved readability and maintainability
  - Created SKILL.md (main entry point, 291 lines)
  - Created PHASE-0-PREPARATION.md (385 lines)
  - Created PHASE-1-ANALYSIS.md (56 lines)
  - Created PHASE-2-BREAKDOWN.md (81 lines)
  - Created PHASE-3-QUESTIONS.md (293 lines)
  - Created PHASE-4-UPDATE.md (402 lines)
  - Created PHASE-5-VERIFICATION.md (123 lines)
  - Created ADVANCED-USAGE.md (112 lines)
  - Created EXAMPLES.md (212 lines with 3 practical examples)
  - Reduced learning time from 30-60 minutes to ~5 minutes (Quick Start)
  - Improved troubleshooting with dedicated sections
  - Enhanced maintainability with modular structure
  - Added comprehensive navigation links between all files

## [3.1.2] - 2026-02-08

### Added

- **todo-task-planning**: Implemented comprehensive 3-layer defense system to prevent Phase 1-2 skip
  - Added [CRITICAL - MUST NOT SKIP] warnings in Phase 1-2 sections (lines 652-769)
  - Added Phase 4 Entrance Gate with prerequisite verification (lines 1061-1115)
  - Added runtime Phase 1-2 skip detection logic (lines 1198-1306)
  - Added Integration Verification logic (lines 1308-1393)
  - Added execution flow diagram (lines 156-209)
  - Added Phase Requirement Markers table (lines 211-220)
  - Added Critical Phase Transition Rules (lines 226-255)

### Fixed

- **todo-task-planning**: Resolved issues with existing progress loss, duplicate tasks, and question history loss
  - Prevents loss of existing task progress during TODO.md updates
  - Prevents duplicate task creation
  - Preserves question/answer history

## [3.1.1] - 2026-02-07

### Fixed

- Standardized skill descriptions to English for consistency
  - Updated `commit` skill description from Japanese to English
  - Updated `pull-request` skill description from Japanese to English

## [3.1.0] - 2026-02-07

### Added

- **Skill Configuration**: Added `model` parameter to skill frontmatter for model selection control
  - Added `model: Haiku` to skills/key-guidelines/SKILL.md
  - Added `model: Haiku` to skills/pull-request/SKILL.md
  - Added `model: Haiku` to skills/todo-output-template/SKILL.md
  - Enables explicit model selection for skills based on complexity and resource requirements

### Changed

- **project-manager skill**: Simplified description for better clarity and conciseness
  - Condensed verbose context examples while preserving key information
  - Improved readability and understanding of skill purpose

## [3.0.10] - 2026-02-07

### Changed

- **Documentation**: Replaced [@docs] placeholders with skill references
  - Updated skills/todo-task-planning/SKILL.md: Replaced `[@docs]` placeholder with skill references
    - Added references to: todo-task-run skill, key-guidelines skill
  - Updated skills/todo-task-run/SKILL.md: Replaced `[@docs]` placeholder with skill references
    - Added references to: todo-task-planning skill, key-guidelines skill
  - All links verified: relative paths validated, all target files confirmed to exist, zero broken links
  - Improved documentation discoverability by providing direct access to related skills

## [3.0.9] - 2026-02-07

### Changed

- **Documentation**: Removed obsolete `--no-pr` and `--no-push` option descriptions from todo-task-run documentation
  - Updated README.md and README.ja.md option tables and usage examples
  - Removed deprecated option examples and explanations from todo-task-development-guide.jp.md (140+ removals)
  - These options were already removed in v2.0.0 (2026-01-27) but documentation was not fully updated
  - CHANGELOG.md v2.0.0 historical record preserved for migration reference

## [3.0.8] - 2026-02-07

### Changed

- **Documentation**: Standardized terminology across all skill documentation files
  - Unified "agent" terminology to proper context-specific terms:
    - Task tool subagents: "agent" → "subagent" (Explore, Plan, general-purpose)
    - Skill tool skills: "agent" → "skill" (git-operations-specialist, project-manager)
  - Updated section headers for consistency:
    - `## Agents` → `## Dependencies` (in commit, micro-commit, pull-request skills)
    - `## Agent Instructions` → `## Instructions` (in commit, micro-commit skills)
  - Updated skills/commit/SKILL.md: 3 terminology updates
  - Updated skills/micro-commit/SKILL.md: 3 terminology updates
  - Updated skills/pull-request/SKILL.md: 4 terminology updates
  - Updated skills/todo-task-planning/SKILL.md: 40+ terminology updates including:
    - Multi-Agent Orchestration → Multi-Subagent Orchestration
    - Phase headers (Explore Agent → Explore Subagent, Plan Agent → Plan Subagent)
    - Agent responsibility → Subagent responsibility
    - Agent Execution → Subagent Execution
  - Updated skills/todo-task-run/SKILL.md: 10+ terminology updates including:
    - Agent Classification Rules → Subagent Classification Rules
    - agent type → subagent type
  - Ensures consistent and accurate terminology aligned with Claude Code architecture

## [3.0.7] - 2026-02-06

### Changed

- **Documentation**: Completed terminology standardization from "agent" to "skill"
  - Updated skills/project-manager/SKILL.md (3 occurrences in description field)
  - Updated skills/todo-task-planning/SKILL.md (8 references to "project-manager agent")
  - Updated skills/git-operations-specialist/SKILL.md (2 occurrences)
  - This completes the standardization work started in commit 1625667
  - Ensures consistent terminology across all skill documentation

## [3.0.6] - 2026-02-06

### Changed

- **SKILL.md Documentation**: Removed Japanese language requirement constraints from skill documentation
  - Removed "日本語で記述する" requirement from `skills/commit/SKILL.md`
  - Removed "日本語で記述する" requirement from `skills/micro-commit/SKILL.md`
  - Removed "日本語で記述する" requirement from `skills/project-manager/SKILL.md`
  - Skills now support language-agnostic documentation

## [3.0.5] - 2026-02-06

### Changed

- **SKILL.md Documentation**: Added structured `arguments` metadata to skill frontmatter
  - Added `arguments` section to `skills/todo-task-planning/SKILL.md` with detailed definitions for `file_path`, `pr`, and `branch` parameters
  - Added `arguments` section to `skills/todo-task-run/SKILL.md` with definition for `file_path` parameter
  - Added argument display templates using `$ARGUMENTS` variable for better user experience

## [3.0.4] - 2026-02-06

### Changed

- **Documentation**: Updated terminology from "agent" to "skill" for git-operations-specialist across documentation
  - Updated skills/pull-request/SKILL.md (2 locations)
  - Updated skills/micro-commit/SKILL.md (3 locations, including "Task tool" → "Skill tool")
  - Updated skills/commit/SKILL.md (3 locations, including "Task tool" → "Skill tool")
  - Updated CONTRIBUTING.md commit message example
  - Updated README.md and README.ja.md directory structure comments

## [3.0.3] - 2026-02-06

### Changed

- **todo-task-planning**: Enhanced file update enforcement from optional to mandatory
  - Changed $ARGUMENTS file update requirement from "Optional" to "MANDATORY"
  - Added critical verification steps in Phase 4 to ensure file update completion
  - Added $ARGUMENTS file update verification checklist in Phase 5 reporting
  - Clarified that file update failure is considered a critical execution error
  - Emphasized that $ARGUMENTS file update is the core purpose of the command

### Fixed

- **Project Structure**: Added `.claude/skills` symbolic link pointing to `../skills` directory
  - Enables access to skills from `.claude` directory context
  - Improves compatibility with Claude Code skill discovery mechanism

## [3.0.2] - 2026-02-06

### Changed

- **todo-task-run skill**: Removed progress tracking file feature and migrated to TODO.md-based context recording
  - Removed automatic creation of `/docs/memory/todo-task-run-progress.md`
  - Task execution context now recorded directly in TODO.md task notes
  - Updated Memory File Initialization to remove progress file references (7 locations)
  - Updated Task Execution Template to use TODO.md for context accumulation
  - Updated Error Handling Protocol to record blockers in TODO.md format with emoji markers
  - Deleted physical progress tracking files (`todo-task-run-progress.md`, `todo-task-run-progress-final.md`)
  - Simplified workflow by consolidating all task information (definitions, status, context, decisions, blockers) into single TODO.md file
  - No changes to command interface - `/todo-task-run` usage remains the same

### Fixed

- **.claude/rules**: Corrected paths to match root-level plugin structure
  - Updated `plugin-versioning.md` paths from `plugins/**/*` to `.claude-plugin/plugin.json`, `CHANGELOG.md`, and `skills/**/*`
  - Updated `documentation.md` paths to remove incorrect `plugins/*/README*.md` references
  - This project uses root-level `.claude-plugin/plugin.json` instead of `plugins/*` subdirectory structure

## [3.0.1] - 2026-02-05

### Changed

- **Documentation**: Removed `cccp:` plugin prefix from all documentation
  - Updated 14 Markdown files (442 pattern replacements)
  - Removed `/cccp:` prefix from slash commands (204 instances → `/`)
  - Removed `cccp:` prefix from agent/skill references (238 instances)
  - Maintained consistency across README, CHANGELOG, skill definitions, and guides
  - No functional changes - command invocation syntax remains backward compatible

## [3.0.0] - 2026-02-04

### Breaking Changes

- **Repository Structure**: Migrated all agents and commands to unified skills architecture
  - Moved `agents/git-operations-specialist.md` → `skills/git-operations-specialist/SKILL.md`
  - Moved `agents/project-manager.md` → `skills/project-manager/SKILL.md`
  - Moved `commands/commit.md` → `skills/commit/SKILL.md`
  - Moved `commands/micro-commit.md` → `skills/micro-commit/SKILL.md`
  - Moved `commands/pull-request.md` → `skills/pull-request/SKILL.md`
  - Moved `commands/todo-task-planning.md` → `skills/todo-task-planning/SKILL.md`
  - Moved `commands/todo-task-run.md` → `skills/todo-task-run/SKILL.md`
  - Deleted `agents/` directory
  - Deleted `commands/` directory

### Changed

- **plugin.json**: Simplified configuration to use skills-only architecture
  - Removed `agents` array (now part of skills)
  - Removed `commands` array (now part of skills)
  - Retained `skills` directory reference

### Technical Details

- **Agent Skills Compliance**: Fully compliant with [Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) open standard
- **Agent Skills**: Converted agents to skills with `context: fork` frontmatter
- **Command Skills**: Converted commands to skills with usage documentation in body
- **Frontmatter Updates**:
  - Removed obsolete fields: `mode`, `arguments` (moved to body)
  - Added required fields: `name`, `user-invocable: true`
  - Added optional fields: `argument-hint`, `context: fork`
  - Preserved functional fields: `trigger_words`, `model` (user decision)
  - Arguments documentation moved from frontmatter to body

### Migration Guide

- **For Plugin Users**: No action required - command invocation syntax (`/command-name`) remains unchanged
- **For Plugin Developers**: Update any direct file path references to new `skills/` structure

## [2.2.0] - 2026-01-31

### Added

- **git-operations-specialist**: Enhanced Japanese language support with additional trigger words
  - Added "コミット" (commit) trigger word
  - Added "プッシュ" (push) trigger word
  - Added "プルリク" (pull request, short form) trigger word
  - Added "プルリクエスト" (pull request, full form) trigger word

## [2.1.2] - 2026-01-29

### Changed

- **todo-task-run**: Enhanced task runner guidelines to emphasize delegation focus
  - Added explicit guidance to delegate individual work to sub-agents as much as possible
  - Refined critical execution policy description for clarity

## [2.1.1] - 2026-01-29

### Removed

- **todo-task-planning**: Removed obsolete backup file `commands/todo-task-planning.md.backup`
  - Cleanup of temporary backup file that is no longer needed

## [2.1.0] - 2026-01-29

### Added

- **todo-output-template**: New skill file for TODO.md output template examples
  - Complete 82-line template with structure documentation
  - Accessible via `/todo-output-template` command
  - Extracted from todo-task-planning.md for better maintainability (commit 02b3b5d)

### Changed

- **todo-task-planning**: Simplified documentation from 1,270 lines to 974 lines (23.3% reduction)
  - TypeScript implementation examples reduced by 79.6% (203 lines) while preserving conceptual patterns
    - Explore Agent example: 42→13 lines (commit c034686)
    - Plan Agent example: 115→15 lines (commit 623179a)
    - Project Manager Agent example: 98→24 lines (commit 24addd2)
  - Verification matrices consolidated from 4 matrices to 2 checklists (23 lines reduction) (commit 743665e)
  - Output template replaced with reference to new skill file (72 lines reduction) (commit e7cd820)
  - All critical sections preserved: Phase 0.5 requirements, security patterns, essential verification points

## [2.0.1] - 2026-01-28

### Fixed

- **todo-task-planning**: Phase 3 Step 9 (AskUserQuestion execution) now enforced by triple-layer verification mechanism
  - Layer 1: Enhanced execution conditions with clear IF-THEN-ELSE flow (commit 810d843)
  - Layer 2: Added Phase 4 entrance guard with mandatory checkpoint verification (commit 00e038b)
  - Layer 3: Added Phase 5 comprehensive validation framework with recovery procedures (commit 235035c)
- **todo-task-planning**: Prevented proceeding to Phase 4 without proper question processing
  - Phase 4 now includes entrance guard that blocks execution if AskUserQuestion was not executed
  - Added validation checkpoint requiring user_answered: true before TODO.md updates

## [2.0.0] - 2026-01-27

### Breaking Changes

- **todo-task-run**: Removed PR and git push functionality - now a pure generic task runner
  - Removed `--no-pr` and `--no-push` command arguments
  - Removed PR creation/update functionality (use `/pull-request` separately)
  - Removed git push operations from task execution workflow
  - Removed git fetch from initial setup
  - Removed `git-operations-specialist` agent type from classification rules
  - Removed `/micro-commit` usage from task completion procedures

### Changed

- **todo-task-run**: Simplified to focus purely on task execution
  - Description changed from "Execute tasks from TODO file and create pull request" to "Execute tasks from TODO file - Generic task runner"
  - Renamed "Management phase" to "Execution phase" in workflow documentation
  - Agent classification rules now only include Implementation Tasks and Investigation Tasks
  - Removed Task Execution Examples section (Git operations, implementation, investigation examples)
  - Simplified error recovery to focus on forward-only changes without commit references
  - Japanese labels changed to English (e.g., "Git操作タスク" → removed, "実装タスク" → "Implementation Tasks")

## [1.0.5] - 2026-01-26

### Changed

- **todo-task-planning**: Clarified file creation responsibility and timeline in Phase 0 and Phase 4
  - Added explicit separation of agent vs main executor responsibilities
  - Agents return data as variables (exploration_results, planning_results, strategic_plan)
  - Main Claude executor creates persistent docs/memory files in Phase 4 using Write tool
  - Added detailed file creation instructions in Phase 4 step 9
  - Added file verification procedures in Phase 0.5 with error recovery actions
  - Clarified that strategic_plan is not saved to disk (used as intermediate data)
  - Improved docs/memory files creation reporting requirements in Phase 5

## [1.0.4] - 2026-01-25

### Fixed

- **todo-task-planning**: Removed automatic git commit from planning phase
  - Removed Phase 4 Step 10 which executed micro-commit after TODO.md update
  - Planning command now focuses purely on task planning without git operations
  - Git operations remain the responsibility of todo-task-run command during task execution
  - Ensures planning commands don't modify git history unexpectedly

## [1.0.3] - 2026-01-24

### Fixed

- **todo-task-run**: Fixed critical issue where session would terminate prematurely with incomplete tasks remaining
  - Added explicit execution policy requiring continuation until ALL tasks are marked complete
  - Added mandatory TODO.md re-read and incomplete task check after each task completion
  - Added session continuation rules preventing premature termination
  - Added Final Completion Process prerequisite check to verify all tasks are complete
  - Added blocker resolution continuation procedure
  - Added execution guarantee documentation clarifying that all tasks will be executed
  - Tasks will now continue executing sequentially until completion or blocker is encountered

## [1.0.2] - 2026-01-23

### Fixed

- **todo-task-planning**: Fixed critical issue where AskUserQuestion tool was not being executed despite being documented as mandatory
  - Added explicit Phase 3 step 9: "AskUserQuestion Tool Execution (MANDATORY BEFORE PHASE 4)"
  - Added mandatory precondition to Phase 4 requiring all questions to be answered before proceeding
  - Added AskUserQuestion execution verification in Phase 5 (steps 11 and 12)
  - Questions will now always be presented to users via AskUserQuestion tool instead of only being written to TODO files
  - User responses will be recorded in `docs/memory/questions/YYYY-MM-DD-[feature]-answers.md`

## [1.0.0] - 2026-01-18

### Breaking Changes

- **todo-task-run**: Removed planning functionality from execution command
  - Planning responsibilities moved exclusively to `todo-task-planning` command
  - `todo-task-run` now focuses solely on executing pre-created TODO.md files
  - Users must use two-phase workflow: planning → execution

### Changed

- **todo-task-run**: Clarified execution-only mode with explicit `mode: run` declaration
- **todo-task-run**: Removed Task Creation section and TDD conversion guidelines
- **todo-task-run**: Removed Investigation Results Storage Guidelines section
- **todo-task-run**: Simplified setup procedure to focus on TODO.md input contract

### Added

- **todo-task-run**: Command Overview section explaining execution mode and relationship with todo-task-planning
- **todo-task-run**: Prerequisites section documenting TODO.md input contract
- **todo-task-run**: Error Handling section with hybrid investigation approach
- **README**: Two-phase workflow documentation with clear planning/execution separation
- **README**: Workflow diagram showing complete task lifecycle

### Removed

- **todo-task-run**: Planning-related guidelines (moved to todo-task-planning)
- **todo-task-run**: Investigation guidelines (replaced with hybrid approach in Error Handling)

## [0.4.1] - 2025-01-XX

### Added

- Initial version with git-operations-specialist agent
- commit, micro-commit, pull-request commands
- todo-task-planning and todo-task-run commands
- project-manager agent
- key-guidelines skill

[2.0.1]: https://github.com/gendosu/gendosu-claude-plugins/compare/v2.0.0...v2.0.1
[2.0.0]: https://github.com/gendosu/gendosu-claude-plugins/compare/v1.0.5...v2.0.0
[1.0.0]: https://github.com/gendosu/gendosu-claude-plugins/compare/v0.4.1...v1.0.0
[0.4.1]: https://github.com/gendosu/gendosu-claude-plugins/releases/tag/v0.4.1
