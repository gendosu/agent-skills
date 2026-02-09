# Phase 1: File Analysis

[← Phase 0](PHASE-0-PREPARATION.md) | [Main](SKILL.md) | [Next: Phase 2 →](PHASE-2-BREAKDOWN.md)

---

### Phase 1: Thorough File Analysis and Existing Status Confirmation

**【CRITICAL - MUST NOT SKIP】**

**⛔ DO NOT PROCEED TO PHASE 4 WITHOUT COMPLETING PHASE 1 ⛔**

**WARNING: Skipping Phase 1-2 will cause serious problems:**
- **Loss of Existing Progress**: Existing tasks and progress status will be overwritten
- **Duplicate Tasks**: Same tasks will be created multiple times, causing confusion
- **Context Loss**: Critical information from $ARGUMENTS file will be ignored
- **Workflow Violation**: Phase 0 → Phase 4 direct transitions are PROHIBITED

**You MUST complete Phase 1-2 before proceeding to Phase 4, even if Phase 0 has been completed.**

**🔍 Starting Phase 1: Existing TODO.md Analysis**
**This phase is MANDATORY to preserve existing progress.**

1. **$ARGUMENTS File Reading**
   - Read the specified file and analyze its content in detail
   - Confirm existing tasks, questions, and progress status
   - Detect changes since the last execution
   - Confirm the progress status of related existing tasks
   - Identify duplicate tasks and related tasks

**Phase 1 Data Extraction Checklist:**
- [ ] `existingTasks`: Existing task list extracted from $ARGUMENTS file
- [ ] `taskProgress`: Progress status categorized (completed, inProgress, pending)
- [ ] `existingQuestions`: Existing question list extracted from $ARGUMENTS file
- [ ] `duplicateTasks`: Duplicate task identification results

**✅ Phase 1 Completion Confirmation:**
```
✅ Phase 1 Completed:
- Extracted {N} existing tasks
- Identified {M} completed tasks
- Found {K} duplicate tasks
Proceeding to Phase 2...
```

2. **Utilizing Phase 0 Results**
   - **Referencing Exploration Results**
     - Check key files, patterns, dependencies from `exploration_results` variable
     - Reference `docs/memory/explorations/YYYY-MM-DD-[feature]-exploration.md`
     - Utilize research results conducted by Explore subagent in Phase 0.2
   - **Duplicate Check**: Check existing research results in docs/memory to avoid duplicate research
   - **Additional Research**: Conduct supplementary research if information is missing from Phase 0

---

[← Phase 0](PHASE-0-PREPARATION.md) | [Main](SKILL.md) | [Next: Phase 2 →](PHASE-2-BREAKDOWN.md)
