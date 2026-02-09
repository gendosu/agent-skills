# Phase 4: File Update

[← Phase 3](PHASE-3-QUESTIONS.md) | [Main](SKILL.md) | [Next: Phase 5 →](PHASE-5-VERIFICATION.md)

---

### Phase 4: $ARGUMENTS File Update

**⚠️ VERIFICATION REQUIRED**: Before proceeding, execute all verification protocols defined in [VERIFICATION-PROTOCOLS.md](VERIFICATION-PROTOCOLS.md):
- [Phase 4 Entrance Gate Verification](VERIFICATION-PROTOCOLS.md#phase-4-entrance-gate-verification)
- [Phase 1 Skip Detection](VERIFICATION-PROTOCOLS.md#phase-1-skip-detection)
- [Phase 2 Skip Detection](VERIFICATION-PROTOCOLS.md#phase-2-skip-detection)
- [Integration Verification](VERIFICATION-PROTOCOLS.md#integration-verification)

**✅ Only proceed to Phase 4 execution steps below when ALL verification checks pass.**

---

**⚠️ MANDATORY PRECONDITION**: All questions extracted in Phase 3 MUST be answered via AskUserQuestion tool before starting this phase. If questions exist but were not answered, STOP and return to Phase 3 step 9.

**🚨 CRITICAL REQUIREMENT**: This phase MUST complete with the $ARGUMENTS file successfully updated. File update is NOT optional - it is the primary output of this command. Failure to update the file is a critical execution failure.

#### File Creation Responsibility and Timeline

**🚨 CRITICAL: docs/memory Files Must Be Created in This Phase**

The following files MUST be created by the Main Claude executor (NOT by subagents) in Phase 4:

1. **Exploration results file** (from Phase 0.2):
   - [ ] **Create** `docs/memory/explorations/YYYY-MM-DD-[feature]-exploration.md`
   - Source: `exploration_results` variable returned by Explore subagent
   - Tool: Use Write tool
   - Format: Structured markdown with sections: Summary, Key Discoveries, Patterns, Tech Stack, Blockers, Recommendations

2. **Planning results file** (from Phase 0.3):
   - [ ] **Create** `docs/memory/planning/YYYY-MM-DD-[feature]-plan.md`
   - Source: `planning_results` variable returned by Plan subagent
   - Tool: Use Write tool
   - Format: Structured markdown with sections: Approach, Task Breakdown, Critical Files, Trade-offs, Risks, Feasibility

3. **User answers file** (from Phase 3, if AskUserQuestion was executed):
   - [ ] **Create** `docs/memory/questions/YYYY-MM-DD-[feature]-answers.md` (if user questions existed)
   - Source: User responses from AskUserQuestion tool
   - Tool: Use Write tool
   - Format: Q&A format with questions and selected answers

**Timeline:**
- **Phase 0.2-0.4**: Subagents return data as variables (`exploration_results`, `planning_results`, `strategic_plan`)
- **Phase 0.5**: Verify subagent completion and data variables exist
- **👉 Phase 4 (THIS PHASE)**: Main Claude executor creates persistent docs/memory files using Write tool
- **Phase 5**: Verify file creation and report to user

**Why Subagents Cannot Create Files:**
- Task tool subagents run in isolated processes
- Subagent-created files do not persist to Main Claude executor's filesystem
- Main Claude executor must explicitly use Write tool to create persistent files

9. **Create docs/memory Files (EXECUTE FIRST)**
    - **⚠️ MANDATORY FIRST STEP**: Before updating $ARGUMENTS file, create all docs/memory files
    - [ ] **Create exploration file** using Write tool:
      - Path: `docs/memory/explorations/YYYY-MM-DD-[feature]-exploration.md`
      - Source data: `exploration_results` variable from Phase 0.2
      - Format: Markdown with sections: Summary, Key Discoveries, Patterns, Tech Stack, Blockers, Recommendations
    - [ ] **Create planning file** using Write tool:
      - Path: `docs/memory/planning/YYYY-MM-DD-[feature]-plan.md`
      - Source data: `planning_results` variable from Phase 0.3
      - Format: Markdown with sections: Approach, Task Breakdown, Critical Files, Trade-offs, Risks, Feasibility
    - [ ] **Create questions file** (if user questions existed) using Write tool:
      - Path: `docs/memory/questions/YYYY-MM-DD-[feature]-answers.md`
      - Source data: User responses from AskUserQuestion tool in Phase 3
      - Format: Q&A format with questions and user's selected answers
    - [ ] **Verify all files were created successfully**
      - Use Bash tool with `ls -la` to confirm file existence
      - Report any file creation failures as CRITICAL ERROR

   **⚠️ PHASE 4 PREREQUISITES CHECK:**

   Before updating TODO.md, execute all prerequisite verifications from [VERIFICATION-PROTOCOLS.md](VERIFICATION-PROTOCOLS.md):
   - [Phase 1 Skip Detection](VERIFICATION-PROTOCOLS.md#phase-1-skip-detection)
   - [Phase 2 Skip Detection](VERIFICATION-PROTOCOLS.md#phase-2-skip-detection)

   **✅ VERIFICATION PASSED**: Only proceed to step 10 (TODO.md update) when ALL data structure validations pass.

  **🔍 INTEGRATION VERIFICATION - Pre-Update Data Quality Check:**

  Immediately before writing to $ARGUMENTS file (TODO.md), execute integration verification from [VERIFICATION-PROTOCOLS.md](VERIFICATION-PROTOCOLS.md#integration-verification).

  **✅ INTEGRATION VERIFICATION PASSED**: Only proceed to step 10 when all integration checks pass OR user explicitly approves proceeding with warnings.

10. **Thorough Update of $ARGUMENTS File (MANDATORY - MUST BE EXECUTED)**
    - **🚨 CRITICAL**: This step is the CORE PURPOSE of the command and MUST be executed
    - Use Edit or Write tool to update the file specified in $ARGUMENTS parameter
    - If file update fails, report as CRITICAL ERROR and retry
    - **🔀 Branch Creation Task (when --branch or --pr option is specified)**
      - **Add branch creation task as the FIRST task** in the task list section
      - Task format example:
        ```markdown
        ### Phase 0: ブランチ作成 ✅

        - [ ] ✅ **ブランチを作成**
          - コマンド: `git checkout -b [branch_name]`
          - 📋 このブランチで全ての変更をコミット
          - 推定時間: 1分
        ```
      - Replace `[branch_name]` with the actual branch name (specified or auto-generated)
      - Place this section before all other task phases
    - **🔀 PR Creation Task (only when --pr option is specified)**
      - **IMPORTANT**: Add PR creation task as the **LAST task** in the task list ONLY when `--pr` option is specified
      - **IMPORTANT**: Do NOT add PR creation task when only `--branch` is specified
      - Task format example:
        ```markdown
        ### Phase 4: PRとマージ ✅/⏳

        - [ ] ✅ 4.1 PRテンプレートに従ったPR作成
          - [ ] `.github/PULL_REQUEST_TEMPLATE.md` 読み込み
          - [ ] PR本文作成（開発理由、開発内容、影響内容を含む）
          - [ ] `gh pr create --title "..." --body "..."` 実行

        - [ ] ⏳ 4.2 レビューとマージ
          - [ ] チームレビュー待機
          - [ ] 承認後マージ実行 `gh pr merge`
        ```
      - Do not include 📁 file references in PR creation tasks (because it's a Git operation task)
    - **Integrating Phase 0 Results**
      - Update file based on `strategic_plan.checklist_structure`
      - Include links to docs/memory:
        - `docs/memory/explorations/YYYY-MM-DD-[feature]-exploration.md`
        - `docs/memory/planning/YYYY-MM-DD-[feature]-plan.md`
      - Record next actions from `strategic_plan.implementation_recommendations`
    - **Required**: Directly update the $ARGUMENTS file (specified file)
    - **Complete Checklist Format**: Describe all tasks in Markdown checklist format with `- [ ]`
    - **Status Display**: Clearly indicate completed `- [x]`, in progress `- [🔄]`, waiting `- [ ]`
    - **Structured Sections**: Maintain checklist format within each category as well
    - **Nested Subtasks**: Create subtask checklists with indentation (2 spaces)
    - Display implementation feasibility indicators (✅⏳🔍🚧) on tasks
    - Describe confirmation items in checklist format as well
    - **Record Research Trail**: Specify referenced and analyzed file paths (detailed research results saved in docs/memory)
    - **Technical Rationale**: Record technical information that served as the basis for determination (detailed analysis saved in docs/memory)
    - **docs/memory Reference**: Record file paths of related research, analysis, and recommendation results
    - Record progress rate and update date
    - Add links to related documents and files
    - Add structured new sections while preserving existing content

---

[← Phase 3](PHASE-3-QUESTIONS.md) | [Main](SKILL.md) | [Next: Phase 5 →](PHASE-5-VERIFICATION.md)
