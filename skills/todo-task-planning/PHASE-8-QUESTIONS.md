# Phase 8: Question Management

[← Previous: Phase 7](PHASE-7-BREAKDOWN.md) | [Main](SKILL.md) | [Next: Phase 9 →](PHASE-9-UPDATE.md)

---

### Phase 8: Thorough Question Management, User Confirmation, and Specification Recommendations

6. **Question Extraction (Only What Is Necessary to Achieve the Objective)**
   - **Utilizing Phase 0 Strategic Plan**
     - Check extracted questions from `strategic_plan.user_questions`
     - Base on questions identified by project-manager skill in Phase 0.4
   - **🚨 Important Constraint**: Extract only questions that are truly necessary to achieve the objective
   - **Required**: Extract concrete unclear points from the researched files and implementation
   - **Duplicate Question Check**: Check past question history in docs/memory to avoid duplicates
   - Extract new unclear points and questions
   - Confirm the status of existing questions (answered/unanswered)
   - Organize questions by category (specification/technology/UI/UX/other)
   - Analyze the impact scope and urgency of questions
   - **Save Question History**: Save question and answer history in docs/memory/questions
   - **🎯 User Question UI**: When you have questions for the user, ALWAYS use the AskUserQuestion tool
     - `strategic_plan.user_questions` already contains structured options
     - Present questions with clear options for the user to select from
     - Provide 2-4 concrete answer choices with descriptions
     - Use multiSelect: true when multiple answers can be selected
     - Set concise headers (max 12 chars) for each question
     - This provides a better UX than asking questions in plain text
   - **🚨 CRITICAL: Thorough Questioning Protocol**
     - When you have questions or uncertainties, keep asking using AskUserQuestion tool until all doubts are resolved
     - Never proceed with assumptions - always confirm unclear points with the user
     - When multiple interpretations are possible, present options and ask the user to choose
     - Do not move to the next phase until all questions and uncertainties are completely resolved

7. **Evidence-Based Specification Recommendations**
   - **Required**: Present concrete recommended specifications based on research results
   - **Existing Recommendation Check**: Check past recommendations in docs/memory to maintain consistency
   - Recommended plan based on existing codebase patterns
   - Proposal of implementation policy considering technical constraints
   - Comparison and evaluation when there are multiple options
   - Specify recommendation reasons and rationale (including reference files and implementation examples)
   - Provide judgment materials with specified risks and benefits
   - **Save Recommendation History**: Save specification recommendations and rationale in docs/memory/recommendations
   - **🎯 User Question UI for Recommendations**: When presenting multiple options to the user, use AskUserQuestion tool
     - Present each option as a selectable choice with clear descriptions
     - Include pros/cons or trade-offs in the option descriptions
     - This allows users to make informed decisions easily

8. **Structural Update of $ARGUMENTS File**
   - Add new questions
   - Update the status of existing questions
   - Specify confirmation items necessary for task execution
   - Record reference information for the next execution
   - **Record Research Rationale**: Specify referenced files and code (details saved in docs/memory)
   - Record recommended specifications and selection reasons in a structured manner (details saved in docs/memory)
   - **docs/memory Reference Information**: Record file paths of related research and analysis results

9. **AskUserQuestion Tool Execution (MANDATORY BEFORE PHASE 4)**

   **⚠️ CRITICAL EXECUTION POLICY**:

   This step enforces a mandatory question extraction and user interaction checkpoint. You MUST evaluate execution conditions and follow the corresponding workflow.

   ### Execution Condition Decision Flow

   ```
   IF (questions extracted in step 6) THEN
       → CONDITION A: Execute AskUserQuestion tool (MANDATORY)
   ELSE
       → CONDITION B: No questions exist (MANDATORY documentation required)
   END IF
   ```

   ---

   ### CONDITION A: Questions Exist (MANDATORY Execution)

   **Triggers**:
   - `strategic_plan.user_questions` exists and contains questions
   - Questions were extracted during Phase 3 analysis
   - Unclear specifications or multiple valid approaches identified

   **🚨 MANDATORY**: You MUST execute AskUserQuestion tool before proceeding to Phase 8

   **Execution Steps**:
   - [ ] Present each question using AskUserQuestion tool with **required parameters**:
     - `header` (string, max 12 chars): Concise question identifier
     - `question` (string): Clear question text with context
     - `options` (array): 2-4 structured choice objects with:
       - `label` (string): Short option identifier
       - `description` (string): Explain implications of each choice
     - `multiSelect` (boolean): Set `true` when multiple answers can be selected
   - [ ] Wait for user responses - **DO NOT proceed to Phase 8 until answered**
   - [ ] Validate that all questions received responses

   **After Receiving Answers**:
   - [ ] **MANDATORY**: Record user responses in `docs/memory/questions/YYYY-MM-DD-[feature]-answers.md`
     - Format: Structured markdown with question headers, selected options, and rationale
     - Example structure:
       ```markdown
       # User Decisions - [Feature Name]
       Date: YYYY-MM-DD

       ## Question 1: [Header]
       **Selected**: [Option Label]
       **Rationale**: [Why this choice was made]

       ## Question 2: [Header]
       **Selected**: [Option Label(s)]
       **Rationale**: [Decision context]
       ```
   - [ ] Update task planning based on user decisions
   - [ ] Resolve any 🚧 Blocked or 🔍 Research tasks that depended on answers

   **Error Handling**:
   - If AskUserQuestion tool fails → **STOP execution** and report error to user
   - If questions.md file creation fails → Retry once, then escalate to user
   - Do NOT proceed to Phase 8 if any question remains unanswered

   ---

   ### CONDITION B: No Questions (MANDATORY Documentation)

   **⚠️ MANDATORY**: If there are genuinely no questions or uncertainties:
   - [ ] Proceed directly to Phase 8
   - [ ] **REQUIRED**: Document in Phase 5 summary why no questions were needed
   - [ ] Explain what made the requirements clear enough to skip user interaction

   ---

   ### Why This Matters

   **Risks of Skipping Questions**:
   - **Incorrect Assumptions**: Proceeding without clarification can lead to implementing wrong features or using inappropriate technical approaches
   - **Wasted Development Effort**: Building features based on misunderstood requirements results in rework and delays
   - **Context Loss**: Without recorded user decisions, future maintainers cannot understand why specific implementation choices were made
   - **User Misalignment**: Skipping user validation means missed opportunities to catch requirement mismatches early

   This checkpoint ensures alignment between user intent and implementation strategy before significant development effort begins.

---

## 🚨 CRITICAL GATE: MANDATORY PHASE 4 ENTRANCE REQUIREMENTS

**PURPOSE**: Prevent execution of Phase 8 without completing Phase 3 question processing when questions exist.
### Step 1: Check Branch Creation Requirement

**Decision Point**: Determine if branch creation task should be inserted based on argument parsing results from Phase 0.1.

```
IF (HAS_BRANCH_OPTION = true) THEN
    → Execute Step 2: Insert branch creation task
ELSE
    → Skip Step 2, proceed to Step 3
END IF
```

**Variables Referenced**:
- `HAS_BRANCH_OPTION` (boolean): Set in Phase 0.1 Step 2, indicates `--branch` flag presence
- `BRANCH_NAME` (string): Branch name from argument or auto-generated in Phase 0.1 Step 3
- `IS_AUTO_GENERATED` (boolean): Indicates if branch name was auto-generated

---

### Step 2: Insert Branch Creation Task (Conditional)

**Execution Condition**: Only execute when `HAS_BRANCH_OPTION = true`

**Branch Name Determination**:
```
IF (IS_AUTO_GENERATED = false) THEN
    → Use BRANCH_NAME as-is (user provided explicit branch name)
ELSE IF (IS_AUTO_GENERATED = true) THEN
    → Use BRANCH_NAME generated in Phase 0.1 Step 3 (auto-generated from feature description)
END IF
```

**Task Template to Insert**:

```markdown
## Phase 0: Repository Preparation

### 0.1 Create Feature Branch

- [ ] **Create and switch to feature branch**
  - Branch name: `{BRANCH_NAME}`
  - Command: `git checkout -b {BRANCH_NAME}`
  - Verify: Current branch should be `{BRANCH_NAME}`
```

**Insertion Rules**:
- **Location**: Insert BEFORE Phase 1 tasks
- **Deduplication**: If existing Phase 0 tasks exist, REPLACE them with this template
- **Numbering**: Renumber subsequent phases if necessary (Phase 1 becomes Phase 1, etc.)

---

### Step 3: Check PR Creation Requirement

**Decision Point**: Determine if PR creation tasks should be inserted based on argument parsing results from Phase 0.1.

```
IF (HAS_PR_OPTION = true) THEN
    → Execute Step 4: Insert PR creation tasks
ELSE
    → Skip Step 4, finalize task list without PR tasks
END IF
```

**Variables Referenced**:
- `HAS_PR_OPTION` (boolean): Set in Phase 0.1 Step 2, indicates `--pr` flag presence

---

### Step 4: Insert PR Creation Tasks (Conditional)

**Execution Condition**: Only execute when `HAS_PR_OPTION = true`

**Task Template to Insert**:

```markdown
## Phase N+1: Pull Request Creation

### N+1.1 Prepare Pull Request

- [ ] **Read PR template**
  - Location: `.github/pull_request_template.md` or `.github/PULL_REQUEST_TEMPLATE.md`
  - If template exists, use structure for PR description
  - If no template, use standard format: Summary, Changes, Testing

### N+1.2 Create Pull Request

- [ ] **Push changes to remote**
  - Command: `git push -u origin {BRANCH_NAME}`
  - Verify: Remote branch created successfully

- [ ] **Create PR using gh CLI**
  - Command: `gh pr create --title "[Title]" --body "[Description from template]"`
  - Alternative: Use GitHub web interface if gh CLI unavailable
  - Verify: PR created successfully, note PR number

### N+1.3 Wait for Review

- [ ] **Monitor PR status**
  - Check CI/CD pipeline results
  - Address review comments if any
  - Request review from team members if needed
```

**Insertion Rules**:
- **Location**: Insert AFTER all existing task phases (Phase N becomes Phase N, this becomes Phase N+1)
- **Deduplication**: If existing final phase contains PR tasks, REPLACE with this template
- **Phase Number**: Calculate N based on highest existing phase number

---

### Step 5: Conditional Behavior Summary

**Task Insertion Matrix**:

| `HAS_BRANCH_OPTION` | `HAS_PR_OPTION` | Branch Task (Phase 0) | PR Tasks (Phase N+1) | Total Additional Phases |
|---------------------|-----------------|----------------------|---------------------|------------------------|
| `false` | `false` | ❌ Not inserted | ❌ Not inserted | 0 (base task list only) |
| `true` | `false` | ✅ Inserted | ❌ Not inserted | +1 (Phase 0 added) |
| `false` | `true` | ❌ Not inserted | ✅ Inserted | +1 (Phase N+1 added) |
| `true` | `true` | ✅ Inserted | ✅ Inserted | +2 (Phase 0 and N+1 added) |

**Branch Name Handling**:
- **User-provided name** (`IS_AUTO_GENERATED = false`): Use `BRANCH_NAME` exactly as provided
- **Auto-generated name** (`IS_AUTO_GENERATED = true`): Use `BRANCH_NAME` generated in Phase 0.1 Step 3

**Deduplication Strategy**:
- Check for existing Phase 0 before inserting branch task → Replace if exists
- Check for existing final phase with PR tasks → Replace if exists
- Never create duplicate phases with same functionality

### Action on Failure

**IF Checkpoint FAILS** (questions exist but AskUserQuestion was not executed):

1. **STOP immediately** - Do NOT proceed to Phase 8
2. **Return to Phase 3 Step 9** - Execute CONDITION A as defined in Phase 3 Step 9
3. **Execute AskUserQuestion tool** - Present all questions to user
4. **Wait for responses** - Do NOT continue until all questions are answered
5. **Create questions.md file** - Document user responses in `docs/memory/questions/`
6. **Re-verify** - Return to this checkpoint and verify PASS criteria

**WHY THIS MATTERS**: Skipping user question validation causes cascading failures in Phase 5 verification (questions.md file will be missing when expected) and risks misalignment between implementation and user intent.

---

---

[← Previous: Phase 7](PHASE-7-BREAKDOWN.md) | [Main](SKILL.md) | [Next: Phase 9 →](PHASE-9-UPDATE.md)
