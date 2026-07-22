# Phase 5: File Analysis

[← Previous: Phase 4](PHASE-5-VERIFICATION.md) | [Main](SKILL.md) | [Next: Phase 6 →](PHASE-7-BREAKDOWN.md)

---

### Phase 5: Thorough File Analysis and Existing Status Confirmation

**【CRITICAL - MUST NOT SKIP】**

**⛔ DO NOT PROCEED TO PHASE 7 WITHOUT COMPLETING PHASE 4 ⛔**

**WARNING: Skipping Phase 4-5 will cause serious problems:**
- **Loss of Existing Progress**: Existing tasks and progress status will be overwritten
- **Duplicate Tasks**: Same tasks will be created multiple times, causing confusion
- **Context Loss**: Critical information from $ARGUMENTS file will be ignored
- **Workflow Violation**: Phase 3 → Phase 7 direct transitions are PROHIBITED

**You MUST complete Phase 4-5 before proceeding to Phase 7, even if Phase 0-3 have been completed.**

**🔍 Starting Phase 5: Existing TODO.md Analysis**
**This phase is MANDATORY to preserve existing progress.**

1. **$ARGUMENTS File Reading**
   - Read the specified file and analyze its content in detail
   - Confirm existing tasks, questions, and progress status
   - Detect changes since the last execution
   - Confirm the progress status of related existing tasks
   - Identify duplicate tasks and related tasks

**Phase 5 Data Extraction Checklist:**
- [ ] `existingTasks`: Existing task list extracted from $ARGUMENTS file
- [ ] `taskProgress`: Progress status categorized (completed, inProgress, pending)
- [ ] `existingQuestions`: Existing question list extracted from $ARGUMENTS file
- [ ] `duplicateTasks`: Duplicate task identification results

**✅ Phase 5 Completion Confirmation:**
```
✅ Phase 5 Completed:
- Extracted {N} existing tasks
- Identified {M} completed tasks
- Found {K} duplicate tasks
Proceeding to Phase 6...
```

2. **Utilizing Phase 0-3 Results**
   - **Referencing Exploration Results**
     - Check key files, patterns, dependencies from `exploration_results` variable (from Phase 2)
     - Reference `docs/memory/explorations/YYYY-MM-DD-[feature]-exploration.md`
     - Utilize research results conducted by Explore subagent in Phase 2
   - **Duplicate Check**: Check existing research results in docs/memory to avoid duplicate research
   - **Additional Research**: Conduct supplementary research if information is missing from Phase 0-3

---

[← Previous: Phase 4](PHASE-5-VERIFICATION.md) | [Main](SKILL.md) | [Next: Phase 6 →](PHASE-7-BREAKDOWN.md)
