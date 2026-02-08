# Phase 4: File Update

[← Phase 3](PHASE-3-QUESTIONS.md) | [Main](SKILL.md) | [Next: Phase 5 →](PHASE-5-VERIFICATION.md)

---

### Phase 4: $ARGUMENTS File Update

**🚨 PREREQUISITES: Phase 1 AND Phase 2 AND Phase 3 MUST BE COMPLETED**

This phase can ONLY execute after ALL previous phases (0-3) have completed successfully. Direct transitions from Phase 0, 1, or 2 to Phase 4 are PROHIBITED.

#### 🔐 Phase 4 Entrance Gate: Mandatory Verification Checklist

**BEFORE executing ANY Phase 4 steps, verify ALL of the following conditions are met:**

- [ ] **Phase 0 Completion Verification**
  - ✅ `exploration_results` variable exists (from Phase 0.2 Explore subagent)
  - ✅ `planning_results` variable exists (from Phase 0.3 Plan subagent)
  - ✅ `strategic_plan` variable exists (from Phase 0.4 Todo subagent)
  - ⚠️ **CRITICAL**: If ANY Phase 0 variable is missing → **STOP** and re-execute Phase 0

- [ ] **Phase 1 Completion Verification**
  - ✅ `existingTasks` data structure exists (from Phase 1 Task Review)
  - ✅ `taskProgress` analysis exists (from Phase 1 Progress Assessment)
  - ⚠️ **CRITICAL**: If Phase 1 data is missing → **STOP** and execute Phase 1

- [ ] **Phase 2 Completion Verification**
  - ✅ `feasibilityAnalysis` exists (from Phase 2 Feasibility Analysis)
  - ✅ `detailedTasks` breakdown exists (from Phase 2 Task Refinement)
  - ⚠️ **CRITICAL**: If Phase 2 data is missing → **STOP** and execute Phase 2

- [ ] **Phase 3 Completion Verification**
  - ✅ Phase 3 question processing completed:
    - **IF** `strategic_plan.user_questions` exists:
      - ✅ AskUserQuestion tool was executed for ALL questions
      - ✅ User responses were received and recorded
      - ✅ `docs/memory/questions/YYYY-MM-DD-[feature]-answers.md` file creation is ready
    - **ELSE** (no questions):
      - ✅ Phase 3 CONDITION B was acknowledged (proceeding directly to Phase 4)
      - ✅ Reason for no questions will be documented in Phase 5
  - ⚠️ **CRITICAL**: If questions exist but AskUserQuestion was NOT executed → **STOP** and return to Phase 3 Step 9

**⚠️ WARNING MESSAGES - If Phase Dependencies Are Missing:**

```
❌ PHASE 4 ENTRANCE GATE VIOLATION DETECTED

Missing Prerequisites:
- Phase 0 results: [list missing variables]
- Phase 1 results: [list missing data]
- Phase 2 results: [list missing data]
- Phase 3 completion: [status]

⛔ PROHIBITED ACTION: Direct transition from Phase [X] to Phase 4

REQUIRED ACTION:
1. STOP Phase 4 execution immediately
2. Return to Phase [first missing phase number]
3. Complete ALL missing phases sequentially
4. Re-verify entrance gate before proceeding to Phase 4

REFERENCE: See "Phase Requirement Markers" table (line 211-220)
and "Critical Phase Transition Rules" (line 226-255)
```

**✅ Only proceed to Phase 4 execution steps below when ALL entrance gate checks pass.**

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

   Before updating TODO.md, verify Phase 1-2 completion by checking the following required data structures:

   - [ ] **existingTasks** (from Phase 1) exists - Array of current TODO.md tasks
   - [ ] **taskProgress** (from Phase 1) exists - Progress assessment of existing tasks
   - [ ] **existingQuestions** (from Phase 1) exists - Historical question data
   - [ ] **feasibilityAnalysis** (from Phase 2) exists - Technical feasibility analysis
   - [ ] **detailedTasks** (from Phase 2) exists - Refined task breakdown with implementation details

   **❌ VERIFICATION FAILED**: If any required data from Phase 1 or Phase 2 is missing:

   ```
   ⛔ CRITICAL: Required data from Phase 1 or Phase 2 is missing.
   STOP: You must return to Phase 1 and execute all phases in order.
   DO NOT PROCEED to TODO.md update without completing Phase 1-2.

   Missing data will cause incomplete or incorrect TODO.md updates.
   ```

   **✅ VERIFICATION PASSED**: Only proceed to step 10 (TODO.md update) when ALL data structures exist.

   **🔍 PHASE 1 SKIP DETECTION - Implementation Level Check:**

   Immediately before proceeding to step 10 (TODO.md update), perform the following runtime validation:

   ```typescript
   // Phase 1 Data Existence Check
   if (!existingTasks) {
     throw new Error("❌ ERROR: Phase 1 was not executed. existingTasks is missing.");
   }

   // Phase 1 Data Structure Validation
   if (!Array.isArray(existingTasks)) {
     throw new Error("❌ ERROR: existingTasks is not an array. Invalid data structure from Phase 1.");
   }

   // Task Progress Validation
   if (!taskProgress) {
     throw new Error("❌ ERROR: Phase 1 was not executed. taskProgress is missing.");
   }
   ```

   **⚠️ DETAILED WARNING MESSAGE - If Phase 1 Was Skipped:**

   When Phase 1 skip is detected (existingTasks is undefined/null), display this comprehensive warning:

   ```
   ❌ CRITICAL ERROR: Phase 1 (Existing TODO.md Analysis) was not executed.

   Missing data structures:
   - existingTasks: undefined/null
   - taskProgress: undefined/null
   - existingQuestions: undefined/null

   CONSEQUENCE OF PROCEEDING WITHOUT PHASE 1:
   1. ⚠️ Loss of existing task progress - All checked [ ] → [x] tasks will be reset
   2. ⚠️ Duplicate tasks - Existing tasks will be duplicated or overwritten
   3. ⚠️ Loss of question/answer history - Historical questions will be lost
   4. ⚠️ Task numbering inconsistency - Task IDs will be regenerated incorrectly
   5. ⚠️ Context loss - Related files and notes will be discarded

   REQUIRED ACTION:
   1. STOP Phase 4 execution immediately
   2. Return to Phase 1 (Existing $ARGUMENTS File Analysis)
   3. Execute Phase 1 to extract existingTasks, taskProgress, and existingQuestions
   4. Continue through Phase 2 and Phase 3 sequentially
   5. Return to Phase 4 only after ALL prerequisite data exists

   REFERENCE:
   - Phase 1 Entrance Gate: Lines 356-396 in SKILL.md
   - Phase 4 Entrance Gate: Lines 1061-1115 in SKILL.md
   - Phase Transition Rules: Lines 226-255 in SKILL.md
   ```

   **✅ DATA STRUCTURE VALIDATION PASSED**: Only proceed to step 10 when existingTasks exists AND is a valid array structure.

   **🔍 PHASE 2 SKIP DETECTION - Implementation Level Check:**

   Immediately before proceeding to step 10 (TODO.md update), perform the following runtime validation:

   ```typescript
   // Phase 2 Data Existence Check
   if (!detailedTasks || !feasibilityAnalysis) {
     throw new Error("❌ ERROR: Phase 2 was not executed. detailedTasks/feasibilityAnalysis is missing.");
   }

   // Phase 2 Data Structure Validation
   if (!Array.isArray(detailedTasks)) {
     throw new Error("❌ ERROR: detailedTasks is not an array. Invalid data structure from Phase 2.");
   }

   // Feasibility Analysis Validation
   if (!feasibilityAnalysis.overall_feasibility) {
     throw new Error("❌ ERROR: feasibilityAnalysis is incomplete. Missing overall_feasibility from Phase 2.");
   }
   ```

   **⚠️ DETAILED WARNING MESSAGE - If Phase 2 Was Skipped:**

   When Phase 2 skip is detected (detailedTasks or feasibilityAnalysis is undefined/null), display this comprehensive warning:

   ```
   ❌ CRITICAL ERROR: Phase 2 (Task Detailing and Integration) was not executed.

   Missing data structures:
   - feasibilityAnalysis: undefined/null
   - detailedTasks: undefined/null

   CONSEQUENCE OF PROCEEDING WITHOUT PHASE 2:
   1. ⚠️ No integration of existing tasks with new tasks - Duplicate work will occur
   2. ⚠️ No duplicate removal - Task list will contain redundant entries
   3. ⚠️ No feasibility analysis - Unrealistic or blocked tasks may be added
   4. ⚠️ No complexity assessment - Time estimates will be missing or inaccurate
   5. ⚠️ No dependency tracking - Task execution order will be incorrect

   REQUIRED ACTION:
   1. STOP Phase 4 execution immediately
   2. Ensure Phase 1 was completed (existingTasks must exist)
   3. Return to Phase 2 (Task Detailing and Integration)
   4. Execute Phase 2 to generate detailedTasks and feasibilityAnalysis
   5. Continue through Phase 3 sequentially
   6. Return to Phase 4 only after ALL prerequisite data exists

   REFERENCE:
   - Phase 2 Entrance Gate: Lines 398-431 in SKILL.md
   - Phase 4 Entrance Gate: Lines 1061-1115 in SKILL.md
   - Phase Transition Rules: Lines 226-255 in SKILL.md
   ```

   **✅ DATA STRUCTURE VALIDATION PASSED**: Only proceed to step 10 when detailedTasks and feasibilityAnalysis exist AND are valid data structures.

  **🔍 INTEGRATION VERIFICATION - Pre-Update Data Quality Check:**

  Immediately before writing to $ARGUMENTS file (TODO.md), perform comprehensive integration verification to ensure data integrity:

  ```typescript
  // 1. Progress Preservation Check - Verify completed tasks are retained
  const preservedProgress = detailedTasks.filter(task =>
    existingTasks.find(et => et.id === task.id && et.status === 'completed')
  );

  // 2. Duplicate Detection - Ensure no redundant tasks remain
  const duplicates = findDuplicates(detailedTasks);
  // findDuplicates logic: Compare task.title and task.description similarity
  // Flag tasks with >80% text similarity as potential duplicates

  // 3. Question Preservation Check - Verify existing questions are retained
  const preservedQuestions = allQuestions.filter(q =>
    existingQuestions.includes(q)
  );

  // 4. New Content Detection - Count newly added items
  const newTasks = detailedTasks.filter(task =>
    !existingTasks.find(et => et.id === task.id)
  );

  const newQuestions = allQuestions.filter(q =>
    !existingQuestions.includes(q)
  );
  ```

  **📊 INTEGRATION VERIFICATION REPORT:**

  Display the following verification summary before proceeding with file update:

  ```
  ✅ Integration Verification Completed:

  📋 Task Integration:
  - Preserved {preservedProgress.length} completed tasks from existing TODO.md
  - Removed {duplicates.length} duplicate tasks
  - Added {newTasks.length} new tasks
  - Total tasks in updated file: {detailedTasks.length}

  ❓ Question Integration:
  - Preserved {preservedQuestions.length} existing questions
  - Added {newQuestions.length} new questions
  - Total questions: {allQuestions.length}

  ✅ Data integrity verified. Proceeding to file update.
  ```

  **⚠️ INTEGRATION VERIFICATION FAILURE HANDLING:**

  If any of the following conditions are detected, HALT and report to user:

  1. **Completed Progress Loss** (`preservedProgress.length < existingCompletedTasks.length`):
     ```
     ❌ ERROR: Progress loss detected!
     - Expected completed tasks: {existingCompletedTasks.length}
     - Preserved completed tasks: {preservedProgress.length}
     - Missing completed tasks: {missingCompletedTasks.map(t => t.title).join(', ')}

     REQUIRED ACTION: Review Phase 2 integration logic to ensure completed tasks are preserved.
     ```

  2. **Excessive Duplicates** (`duplicates.length > 0`):
     ```
     ⚠️ WARNING: {duplicates.length} duplicate tasks detected!
     - Duplicate tasks: {duplicates.map(d => d.title).join(', ')}

     RECOMMENDED ACTION: Review Phase 2 duplicate removal logic.
     User confirmation required before proceeding with duplicates.
     ```

  3. **Existing Questions Lost** (`preservedQuestions.length < existingQuestions.length`):
     ```
     ❌ ERROR: Existing questions were lost during integration!
     - Expected preserved questions: {existingQuestions.length}
     - Actual preserved questions: {preservedQuestions.length}
     - Missing questions: {missingQuestions.join(', ')}

     REQUIRED ACTION: Review Phase 3 question integration logic.
     ```

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
