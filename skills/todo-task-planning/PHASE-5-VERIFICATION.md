# Phase 5: Verification

[← Phase 4](PHASE-4-UPDATE.md) | [Main](SKILL.md)

---

### Phase 5: Thorough Verification and Feedback

11. **Multi-faceted Update Result Verification**
    - **Required**: Reload and confirm the updated file
    - Verify the consistency and completeness of tasks
    - Check for missing or duplicate questions
    - **AskUserQuestion Execution Verification**:
      - [ ] Confirm AskUserQuestion tool was executed for all extracted questions in Phase 3
      - [ ] Verify user responses were recorded in `docs/memory/questions/YYYY-MM-DD-[feature]-answers.md`
      - [ ] If questions existed but AskUserQuestion was NOT executed, this is a CRITICAL ERROR - report to user
      - [ ] If no questions existed, document this fact in Phase 5 summary
    - **Questions Processing Integrity Validation** (references Phase 3 Step 9 + Phase 4 Entrance Guard):
      - [ ] **Cross-reference validation**: Compare questions.md file against AskUserQuestion execution history
      - [ ] **Completeness check**: Verify ALL questions in questions.md have recorded answers
        - ✅ **SUCCESS**: All questions have documented answers → Proceed to Step 11
        - 🚨 **FAILURE**: One or more questions lack answers → Execute recovery procedure below
        - ⚠️ **GRAY ZONE**: Questions marked unnecessary without documentation → Return to Phase 3 Step 9
      - [ ] **Recovery procedure on validation failure**:

        **STEP 1: STOP Processing**
        - [ ] Halt Phase 5 Step 11 immediately - Do NOT proceed to final summary
        - [ ] Suspend all pending verification tasks
        - [ ] Mark current execution as "RECOVERY MODE"

        **STEP 2: DOCUMENT GAP (Evidence Collection)**
        - [ ] Identify missing questions by comparing:
          - Source: `docs/memory/questions/YYYY-MM-DD-[feature]-questions.md` (expected questions)
          - Target: `docs/memory/questions/YYYY-MM-DD-[feature]-answers.md` (recorded answers)
        - [ ] Create gap analysis report with:
          - Total questions count: [N]
          - Answered questions count: [M]
          - Missing questions count: [N - M]
          - Specific missing question IDs/text: [List each unanswered question]
        - [ ] Record gap in execution log with timestamp and context

        **STEP 3: ROLLBACK to Phase 3 Step 9**
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

        **STEP 4: RESUME Phase 5**
        - [ ] Return to Phase 5 Step 11 verification point
        - [ ] Re-execute "Questions Processing Integrity Validation"
        - [ ] Expected result: SUCCESS (all questions answered)
        - [ ] Continue to Step 12 (Comprehensive Execution Summary)

        **Common Failure Scenarios**:
        - **Scenario 1**: AskUserQuestion tool was executed but answers.md file was not created
          - Root cause: File write failure or incorrect file path
          - Recovery: Re-execute AskUserQuestion, verify file creation explicitly
        - **Scenario 2**: Some questions were skipped during Phase 3 execution
          - Root cause: Conditional logic error or incomplete iteration
          - Recovery: Compare questions.md line-by-line against answers.md entries
        - **Scenario 3**: Answers file exists but contains incomplete responses
          - Root cause: User response not properly recorded or partial execution
          - Recovery: Identify incomplete entries, re-ask specific questions
        - **Scenario 4**: Questions file was updated after answers were recorded
          - Root cause: Phase 3 re-execution without Phase 4 entrance guard check
          - Recovery: Use timestamp comparison, re-ask only newly added questions
      - [ ] **Validation Checklist**:
        - [ ] Both questions.md and answers.md files exist (if questions were needed)
        - [ ] Question count equals answer count (verify 1:1 mapping with `grep -c "^##"`)
        - [ ] All answers are complete with no TODO/placeholder text
        - [ ] Question headers match answer headers exactly
        - [ ] answers.md timestamp is newer than questions.md (no new unanswered questions)
        - [ ] **SUCCESS**: All checks pass → Proceed to Step 11
        - [ ] **FAILURE**: Any check fails → Execute recovery procedure
        - [ ] **ANOMALY**: Unexpected state (e.g., answers.md exists without questions.md) → Investigate
    - **Technical Consistency Verification**: Reconfirm whether the proposed tasks are technically executable
    - **Dependency Verification**: Confirm whether dependencies between tasks are correctly set
    - **Research Rationale Verification**: Confirm whether there are any omissions in the recorded research results

12. **Comprehensive Execution Summary Provision**
    - **Research Performance**: Report the number of researched files and directories
    - **Analysis Results**: Report the number of newly created tasks and their classification
    - **Verification Status**: Report identified questions and confirmation items
    - **$ARGUMENTS File Update Verification** (MANDATORY):
      - [ ] **CRITICAL**: Confirm that the $ARGUMENTS file (specified in command argument) was updated in Phase 4
        - Read the file using Read tool to verify the update was successful
        - Compare file modification timestamp to confirm recent update
        - If NOT updated: Report as CRITICAL ERROR - the file update is mandatory
        - If updated: Confirm new tasks, questions, and sections were added correctly
    - **docs/memory Files Creation Report** (MANDATORY):
      - [ ] **Exploration file**: Confirm `docs/memory/explorations/YYYY-MM-DD-[feature]-exploration.md` was created
        - If NOT created: Report as CRITICAL ERROR with explanation
        - If created: Report file size and confirm contents
      - [ ] **Planning file**: Confirm `docs/memory/planning/YYYY-MM-DD-[feature]-plan.md` was created
        - If NOT created: Report as CRITICAL ERROR with explanation
        - If created: Report file size and confirm contents
      - [ ] **Questions file** (if applicable): Confirm `docs/memory/questions/YYYY-MM-DD-[feature]-answers.md` was created (if user questions existed)
        - Report whether this file was needed and created
    - **AskUserQuestion Execution Report** (MANDATORY):
      - Report whether AskUserQuestion tool was executed
      - If executed: Report number of questions asked and answers received
      - If not executed: Explicitly state "No questions required" with justification
      - Report location of recorded answers: `docs/memory/questions/YYYY-MM-DD-[feature]-answers.md`
      - **Questions Processing Integrity Status**:
        - Report validation result: SUCCESS / FAILURE / GRAY ZONE (as defined in Questions Processing Integrity Validation)
        - If FAILURE: Report gap details (missing question count, specific IDs)
        - If recovery executed: Report rollback to Phase 3 Step 9 and re-execution results
        - Confirm 1:1 mapping achieved: Questions count = Answers count
    - **Technical Insights**: Report discovered technical issues, constraints, and opportunities
    - **Recommended Actions**: Concretely specify the next action items
    - **Improvement Proposals**: Propose improvements for iterative execution
    - **Update Confirmation**: Confirm and report that the $ARGUMENTS file has been updated normally
    - **Quality Indicators**: Self-evaluate the thoroughness of research, accuracy of analysis, and practicality of proposals

---

[← Phase 4](PHASE-4-UPDATE.md) | [Main](SKILL.md)
