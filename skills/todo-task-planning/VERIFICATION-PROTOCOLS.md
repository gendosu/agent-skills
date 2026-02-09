# Verification Protocols

[← Main](SKILL.md) | [Phase 4](PHASE-4-UPDATE.md) | [Phase 5](PHASE-5-VERIFICATION.md)

---

## Overview

This document contains all verification protocols, validation checklists, and error recovery procedures used throughout the todo-task-planning skill execution.

**Purpose**: Ensure data integrity, phase completion, and prevent information loss during task planning execution.

**Target Audience**: Main Claude executor during Phase 4 and Phase 5 execution.

---

## Table of Contents

1. [Phase 4 Entrance Gate Verification](#phase-4-entrance-gate-verification)
2. [Phase 1 Skip Detection](#phase-1-skip-detection)
3. [Phase 2 Skip Detection](#phase-2-skip-detection)
4. [Integration Verification](#integration-verification)
5. [Questions Processing Integrity Validation](#questions-processing-integrity-validation)

---

## Phase 4 Entrance Gate Verification

**🚨 PREREQUISITES: Phase 1 AND Phase 2 AND Phase 3 MUST BE COMPLETED**

Phase 4 can ONLY execute after ALL previous phases (0-3) have completed successfully. Direct transitions from Phase 0, 1, or 2 to Phase 4 are PROHIBITED.

### 🔐 Mandatory Verification Checklist

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

### ⚠️ Warning Messages - If Phase Dependencies Are Missing

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

REFERENCE: See "Phase Requirement Markers" table (SKILL.md)
and "Critical Phase Transition Rules" (SKILL.md)
```

**✅ Only proceed to Phase 4 execution steps when ALL entrance gate checks pass.**

---

## Phase 1 Skip Detection

### Prerequisites Check

Before updating TODO.md in Phase 4, verify Phase 1 completion by checking the following required data structures:

- [ ] **existingTasks** (from Phase 1) exists - Array of current TODO.md tasks
- [ ] **taskProgress** (from Phase 1) exists - Progress assessment of existing tasks
- [ ] **existingQuestions** (from Phase 1) exists - Historical question data

### ❌ Verification Failed

If any required data from Phase 1 is missing:

```
⛔ CRITICAL: Required data from Phase 1 is missing.
STOP: You must return to Phase 1 and execute all phases in order.
DO NOT PROCEED to TODO.md update without completing Phase 1.

Missing data will cause incomplete or incorrect TODO.md updates.
```

### ✅ Verification Passed

Only proceed to Phase 4 TODO.md update when ALL data structures exist.

### 🔍 Implementation Level Check

Immediately before proceeding to Phase 4 TODO.md update, perform the following runtime validation:

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

### ⚠️ Detailed Warning Message - If Phase 1 Was Skipped

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
- Phase 1 Entrance Gate: See PHASE-1-ANALYSIS.md
- Phase 4 Entrance Gate: See this document
- Phase Transition Rules: See SKILL.md
```

**✅ DATA STRUCTURE VALIDATION PASSED**: Only proceed when existingTasks exists AND is a valid array structure.

---

## Phase 2 Skip Detection

### 🔍 Implementation Level Check

Immediately before proceeding to Phase 4 TODO.md update, perform the following runtime validation:

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

### ⚠️ Detailed Warning Message - If Phase 2 Was Skipped

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
- Phase 2 Entrance Gate: See PHASE-2-BREAKDOWN.md
- Phase 4 Entrance Gate: See this document
- Phase Transition Rules: See SKILL.md
```

**✅ DATA STRUCTURE VALIDATION PASSED**: Only proceed when detailedTasks and feasibilityAnalysis exist AND are valid data structures.

---

## Integration Verification

### 🔍 Pre-Update Data Quality Check

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

### 📊 Integration Verification Report

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

### ⚠️ Integration Verification Failure Handling

If any of the following conditions are detected, HALT and report to user:

#### 1. Completed Progress Loss

`preservedProgress.length < existingCompletedTasks.length`:

```
❌ ERROR: Progress loss detected!
- Expected completed tasks: {existingCompletedTasks.length}
- Preserved completed tasks: {preservedProgress.length}
- Missing completed tasks: {missingCompletedTasks.map(t => t.title).join(', ')}

REQUIRED ACTION: Review Phase 2 integration logic to ensure completed tasks are preserved.
```

#### 2. Excessive Duplicates

`duplicates.length > 0`:

```
⚠️ WARNING: {duplicates.length} duplicate tasks detected!
- Duplicate tasks: {duplicates.map(d => d.title).join(', ')}

RECOMMENDED ACTION: Review Phase 2 duplicate removal logic.
User confirmation required before proceeding with duplicates.
```

#### 3. Existing Questions Lost

`preservedQuestions.length < existingQuestions.length`:

```
❌ ERROR: Existing questions were lost during integration!
- Expected preserved questions: {existingQuestions.length}
- Actual preserved questions: {preservedQuestions.length}
- Missing questions: {missingQuestions.join(', ')}

REQUIRED ACTION: Review Phase 3 question integration logic.
```

**✅ INTEGRATION VERIFICATION PASSED**: Only proceed when all integration checks pass OR user explicitly approves proceeding with warnings.

---

## Questions Processing Integrity Validation

**References**: Phase 3 Step 9 + Phase 4 Entrance Guard

This validation ensures that all questions extracted in Phase 3 were properly answered through the AskUserQuestion tool.

### Validation Checklist

- [ ] **Cross-reference validation**: Compare questions.md file against AskUserQuestion execution history
- [ ] **Completeness check**: Verify ALL questions in questions.md have recorded answers
  - ✅ **SUCCESS**: All questions have documented answers → Proceed to next step
  - 🚨 **FAILURE**: One or more questions lack answers → Execute recovery procedure below
  - ⚠️ **GRAY ZONE**: Questions marked unnecessary without documentation → Return to Phase 3 Step 9

### Recovery Procedure on Validation Failure

#### STEP 1: STOP Processing

- [ ] Halt Phase 5 verification immediately - Do NOT proceed to final summary
- [ ] Suspend all pending verification tasks
- [ ] Mark current execution as "RECOVERY MODE"

#### STEP 2: DOCUMENT GAP (Evidence Collection)

- [ ] Identify missing questions by comparing:
  - Source: `docs/memory/questions/YYYY-MM-DD-[feature]-questions.md` (expected questions)
  - Target: `docs/memory/questions/YYYY-MM-DD-[feature]-answers.md` (recorded answers)
- [ ] Create gap analysis report with:
  - Total questions count: [N]
  - Answered questions count: [M]
  - Missing questions count: [N - M]
  - Specific missing question IDs/text: [List each unanswered question]
- [ ] Record gap in execution log with timestamp and context

#### STEP 3: ROLLBACK to Phase 3 Step 9

- [ ] Navigate to Phase 3 Step 9: "Questions Extraction and User Interaction"
- [ ] Execute CONDITION A with **ONLY missing questions**:
  - Load unanswered questions from gap analysis
  - Execute AskUserQuestion tool for each missing question
  - Wait for user responses (do NOT proceed without answers)
  - Append new answers to existing answers.md file (preserve previous answers)
- [ ] **Verification after rollback**:
  - Re-run completeness check (questions count = answers count)
  - Confirm 1:1 mapping achieved
  - If still failing → Escalate to user with detailed error report

#### STEP 4: RESUME Phase 5

- [ ] Return to Phase 5 verification point
- [ ] Re-execute "Questions Processing Integrity Validation"
- [ ] Expected result: SUCCESS (all questions answered)
- [ ] Continue to comprehensive execution summary

### Common Failure Scenarios

#### Scenario 1: AskUserQuestion executed but answers.md not created

- **Root cause**: File write failure or incorrect file path
- **Recovery**: Re-execute AskUserQuestion, verify file creation explicitly

#### Scenario 2: Some questions were skipped during Phase 3

- **Root cause**: Conditional logic error or incomplete iteration
- **Recovery**: Compare questions.md line-by-line against answers.md entries

#### Scenario 3: Answers file exists but contains incomplete responses

- **Root cause**: User response not properly recorded or partial execution
- **Recovery**: Identify incomplete entries, re-ask specific questions

#### Scenario 4: Questions file updated after answers recorded

- **Root cause**: Phase 3 re-execution without Phase 4 entrance guard check
- **Recovery**: Use timestamp comparison, re-ask only newly added questions

### Final Validation Checklist

- [ ] Both questions.md and answers.md files exist (if questions were needed)
- [ ] Question count equals answer count (verify 1:1 mapping with `grep -c "^##"`)
- [ ] All answers are complete with no TODO/placeholder text
- [ ] Question headers match answer headers exactly
- [ ] answers.md timestamp is newer than questions.md (no new unanswered questions)
- [ ] **SUCCESS**: All checks pass → Proceed to next verification step
- [ ] **FAILURE**: Any check fails → Execute recovery procedure
- [ ] **ANOMALY**: Unexpected state (e.g., answers.md exists without questions.md) → Investigate

---

## Usage in Phase 4

When executing Phase 4, refer to this document for:

1. **Before Phase 4 execution**: Run "Phase 4 Entrance Gate Verification"
2. **Before TODO.md update**: Run "Phase 1 Skip Detection" and "Phase 2 Skip Detection"
3. **Before file write**: Run "Integration Verification"

**Reference in Phase 4**:
```markdown
**⚠️ VERIFICATION REQUIRED**: Before proceeding, execute all verification protocols defined in [VERIFICATION-PROTOCOLS.md](VERIFICATION-PROTOCOLS.md):
- Phase 4 Entrance Gate Verification
- Phase 1 Skip Detection
- Phase 2 Skip Detection
- Integration Verification
```

---

## Usage in Phase 5

When executing Phase 5, refer to this document for:

1. **AskUserQuestion verification**: Run "Questions Processing Integrity Validation"
2. **File update verification**: Reference entrance gate checks from Phase 4

**Reference in Phase 5**:
```markdown
**⚠️ VERIFICATION REQUIRED**: Execute questions processing integrity validation defined in [VERIFICATION-PROTOCOLS.md](VERIFICATION-PROTOCOLS.md#questions-processing-integrity-validation)
```

---

[← Main](SKILL.md) | [Phase 4](PHASE-4-UPDATE.md) | [Phase 5](PHASE-5-VERIFICATION.md)
