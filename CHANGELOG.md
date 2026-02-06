# Changelog

All notable changes to the CCCP plugin will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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
