# Phase 9: File Update

[← Previous: Phase 8](PHASE-8-QUESTIONS.md) | [Main](SKILL.md) | [Next: Phase 10 →](PHASE-10-VERIFICATION.md)

---

### Phase 9: $ARGUMENTS File Update

**⚠️ VERIFICATION REQUIRED**: Before proceeding, execute all verification protocols defined in [VERIFICATION-PROTOCOLS.md](VERIFICATION-PROTOCOLS.md):
- [Phase 4 Entrance Gate Verification](VERIFICATION-PROTOCOLS.md#phase-4-entrance-gate-verification)
- [Phase 1 Skip Detection](VERIFICATION-PROTOCOLS.md#phase-1-skip-detection)
- [Phase 2 Skip Detection](VERIFICATION-PROTOCOLS.md#phase-2-skip-detection)
- [Integration Verification](VERIFICATION-PROTOCOLS.md#integration-verification)

**✅ Only proceed to Phase 4 execution steps below when ALL verification checks pass.**

---

**⚠️ MANDATORY PRECONDITION**: All questions extracted in Phase 7 MUST be answered via AskUserQuestion tool before starting this phase. If questions exist but were not answered, STOP and return to Phase 7 step 9.

**🚨 CRITICAL REQUIREMENT**: This phase MUST complete with the $ARGUMENTS file successfully updated. File update is NOT optional - it is the primary output of this command. Failure to update the file is a critical execution failure.

#### File Creation Responsibility and Timeline

**🚨 CRITICAL: docs/memory Files Must Be Created in This Phase**

The following files MUST be created by the Main Claude executor (NOT by subagents) in Phase 9:

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

3. **User answers file** (from Phase 7, if AskUserQuestion was executed):
   - [ ] **Create** `docs/memory/questions/YYYY-MM-DD-[feature]-answers.md` (if user questions existed)
   - Source: User responses from AskUserQuestion tool
   - Tool: Use Write tool
   - Format: Q&A format with questions and selected answers

**Timeline:**
- **Phase 0.2-0.4**: Subagents return data as variables (`exploration_results`, `planning_results`, `strategic_plan`)
- **Phase 0.5**: Verify subagent completion and data variables exist
- **👉 Phase 4 (THIS PHASE)**: Main Claude executor creates persistent docs/memory files using Write tool
- **Phase 9**: Verify file creation and report to user

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
      - Source data: User responses from AskUserQuestion tool in Phase 7
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

    - **🔀 Branch Creation Task (Conditional)**

      **Decision Point**: Determine if branch creation task should be inserted based on argument parsing results from Phase 0.1.

      ```
      IF (HAS_BRANCH_OPTION = true) THEN
          → Execute branch creation task insertion
      ELSE
          → Skip branch creation task insertion, proceed to PR check
      END IF
      ```

      **Variables Referenced**:
      - `HAS_BRANCH_OPTION` (boolean): Set in Phase 0.1 Step 2, indicates `--branch` flag presence
      - `BRANCH_NAME` (string): Branch name from argument or auto-generated in Phase 0.1 Step 3
      - `IS_AUTO_GENERATED` (boolean): Indicates whether branch name was auto-generated

      **Branch Name Determination**:
      ```
      IF (IS_AUTO_GENERATED = false) THEN
          → Use BRANCH_NAME as-is (user explicitly specified branch name)
      ELSE IF (IS_AUTO_GENERATED = true) THEN
          → Use BRANCH_NAME generated in Phase 0.1 Step 3 (auto-generated from feature description)
      END IF
      ```

      **Task Template to Insert** (when `HAS_BRANCH_OPTION = true`):

      ```markdown
      ### Phase 0: Branch Creation ✅

      - [ ] ✅ **Create Branch**
        - Branch name: `{BRANCH_NAME}`
        - Command: `git checkout -b {BRANCH_NAME}`
        - Verification: Confirm current branch is `{BRANCH_NAME}`
        - 📋 Commit all changes on this branch
        - Estimated time: 1 minute
      ```

      **Insertion Rules**:
      - **Location**: Insert BEFORE Phase 1 tasks (as Phase 0)
      - **Placeholder Replacement**: Replace `{BRANCH_NAME}` with actual branch name
      - **Deduplication**: If existing Phase 0 task exists, REPLACE with this template
      - **Numbering**: Renumber subsequent phases if necessary (existing Phase 1 remains Phase 1, etc.)

    - **🔀 PR Creation Tasks (Conditional)**

      **Decision Point**: Determine if PR creation tasks should be inserted based on argument parsing results from Phase 0.1.

      ```
      IF (HAS_PR_OPTION = true) THEN
          → Execute PR creation task insertion
      ELSE
          → Skip PR creation task insertion, finalize task list without PR tasks
      END IF
      ```

      **Variables Referenced**:
      - `HAS_PR_OPTION` (boolean): Set in Phase 0.1 Step 2, indicates `--pr` flag presence
      - `BRANCH_NAME` (string): Branch name from argument or auto-generated (used in push command)

      **Task Template to Insert** (when `HAS_PR_OPTION = true`):

      ```markdown
      ### Phase N+1: Pull Request Creation and Merge ✅/⏳

      - [ ] ✅ N+1.1 PR Preparation Following Template
        - [ ] Read `.github/PULL_REQUEST_TEMPLATE.md` or `.github/pull_request_template.md`
        - [ ] If template exists, create PR description following its structure
        - [ ] If no template, use standard format: Summary, Changes, Testing
        - Estimated time: 5 minutes

      - [ ] ✅ N+1.2 Create Pull Request
        - [ ] Push changes to remote: `git push -u origin {BRANCH_NAME}`
        - [ ] Verify remote branch was created successfully
        - [ ] Create PR using gh CLI: `gh pr create --title "[Title]" --body "[Description created from template]"`
        - [ ] Alternative: Use GitHub Web interface if gh CLI is unavailable
        - [ ] Verify PR was created successfully and record PR number
        - Estimated time: 3 minutes

      - [ ] ⏳ N+1.3 Review and Merge
        - [ ] Check CI/CD pipeline results
        - [ ] Address review comments if any
        - [ ] Request review from team members if necessary
        - [ ] After approval, execute merge: `gh pr merge`
        - Estimated time: Variable (depends on review wait time)
      ```

      **Insertion Rules**:
      - **Location**: Insert AFTER all existing task phases (as Phase N+1)
      - **Placeholder Replacement**: Replace `{BRANCH_NAME}` with the actual branch name
      - **Deduplication**: If existing final phase contains PR tasks, REPLACE with this template
      - **Phase Number Calculation**: Calculate N based on highest existing phase number (N+1 becomes the new final phase)
      - **Important**: Do NOT insert when only `--branch` is specified without `--pr`

    - **🔀 Conditional Behavior Summary**

      **Task Insertion Matrix**:

      | `HAS_BRANCH_OPTION` | `HAS_PR_OPTION` | Branch Task (Phase 0) | PR Task (Phase N+1) | Total Additional Phases |
      |---------------------|-----------------|----------------------|---------------------|------------------------|
      | `false` | `false` | ❌ Not inserted | ❌ Not inserted | 0 (base task list only) |
      | `true` | `false` | ✅ Inserted | ❌ Not inserted | +1 (Phase 0 added) |
      | `false` | `true` | ❌ Not inserted | ✅ Inserted | +1 (Phase N+1 added) |
      | `true` | `true` | ✅ Inserted | ✅ Inserted | +2 (Phase 0 and N+1 added) |

      **Branch Name Handling**:
      - **User-provided name** (`IS_AUTO_GENERATED = false`): Use `BRANCH_NAME` as-is
      - **Auto-generated name** (`IS_AUTO_GENERATED = true`): Use `BRANCH_NAME` generated in Phase 0.1 Step 3

      **Deduplication Strategy**:
      - Check for existing Phase 0 before branch task insertion → Replace if exists
      - Check for existing final phase before PR task insertion → Replace if contains PR-related tasks
      - Do not create duplicate phases with identical functionality

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

[← Previous: Phase 8](PHASE-8-QUESTIONS.md) | [Main](SKILL.md) | [Next: Phase 10 →](PHASE-10-VERIFICATION.md)
