# Changelog - 2.x

All notable changes to the CCCP plugin will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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

[2.0.1]: https://github.com/gendosu/gendosu-claude-plugins/compare/v2.0.0...v2.0.1
[2.0.0]: https://github.com/gendosu/gendosu-claude-plugins/compare/v1.0.5...v2.0.0
