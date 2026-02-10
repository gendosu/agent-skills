# Phase 3: project-manager Skill

[← Previous: Phase 2](PHASE-2-PLAN.md) | [Next: Phase 4 →](PHASE-4-VERIFICATION.md)

---

## Overview

Phase 3 calls the project-manager skill to integrate exploration and planning results and organize strategically.

**Critical Output:**
- `strategic_plan`: Structured data containing organized tasks and questions

**[CRITICAL] ONE TASK TOOL PER MESSAGE**:
- Call the project-manager Task tool in THIS message
- **STOP** after calling the Task tool
- **WAIT** for the tool result to arrive in the NEXT message

---

## Prerequisites

**[WARNING] MANDATORY PRECONDITION: Both Phase 1 AND Phase 2 MUST Be Completed First**

**DO NOT proceed with Phase 3 unless ALL of the following are confirmed:**
- [ ] Phase 1 Explore subagent has successfully completed
- [ ] Phase 2 Plan subagent has successfully completed
- [ ] Both `exploration_results` and `planning_results` variables exist
- [ ] No errors occurred during exploration or planning

**Sequential Dependency Chain:**
```
Phase 1 (Explore) → exploration_results
                          ↓
Phase 2 (Plan) → planning_results
                          ↓
Phase 3 (project-manager) → strategic_plan
```

**ONLY after confirming the above, execute the project-manager skill Task tool.**

---

## Execution Steps

### 1. Verify Prerequisites

Confirm both `exploration_results` and `planning_results` exist before calling the Task tool.

### 2. Call project-manager Task Tool

**YOU MUST:**
1. Verify both `exploration_results` and `planning_results` exist
2. Call the project-manager Task tool in this message (pass both results in prompt)
3. **STOP** after calling the Task tool
4. **WAIT** for the tool result to arrive
5. Verify `strategic_plan` contains valid data
6. Proceed to Phase 4 verification

**Task tool execution example**:
```typescript
Task({
  subagent_type: "project-manager",
  description: "Strategic organization for [feature name]",
  prompt: `
    # Strategic Project Planning Request

    ## Context
    Exploration results: ${exploration_results.summary}
    Planning results: ${planning_results.approach_summary}

    ## Goals
    1. Organize tasks by feasibility (✅⏳🔍🚧)
    2. Extract user questions with structured options
    3. Prepare checklist structure with file references (📁) and rationale (📊)
    4. Apply YAGNI principle validation

    ## Deliverables
    - tasks_by_feasibility: {ready, pending, research, blocked}
    - user_questions: Array with options for AskUserQuestion tool
    - checklist_structure: Complete markdown checklist
    - implementation_recommendations: Next actions and quality metrics
  `
})
```

### 3. Saving Results

**Subagent responsibility**:
- Return structured data in variable `strategic_plan` containing:
  - `tasks_by_feasibility`: {ready: [], pending: [], research: [], blocked: []}
  - `user_questions`: Array of question objects with options (for AskUserQuestion tool)
  - `checklist_structure`: Complete markdown checklist format
  - `implementation_recommendations`: Next action items and quality metrics

**Note**: strategic_plan is NOT saved to disk
- **Reason**: Used as intermediate data structure for organizing tasks in Phase 8
- **Persistence**: Strategic plan data (tasks, questions, checklist) are integrated into $ARGUMENTS file in Phase 8
- **Reference**: Exploration and planning files already provide persistent storage of analysis results

---

## Next Steps

After Phase 3 completes successfully, proceed to Phase 4 (Verification) in a NEW message.

---

[← Previous: Phase 2](PHASE-2-PLAN.md) | [Next: Phase 4 →](PHASE-4-VERIFICATION.md)
