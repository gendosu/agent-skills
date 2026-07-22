# Phase 6: Task Breakdown

[← Previous: Phase 5](PHASE-6-ANALYSIS.md) | [Main](SKILL.md) | [Next: Phase 7 →](PHASE-8-QUESTIONS.md)

---

### Phase 6: Thorough Task Analysis, Breakdown, Design, and Strategic Plan

**🔍 Starting Phase 7: Task Detailing and Integration**
**Integrating Phase 1-5 planning results with Phase 5 existing tasks...**

**【CRITICAL - MUST NOT SKIP】**

**⛔ PHASE 1 RESULTS ARE MANDATORY INPUT - CANNOT PROCEED WITHOUT THEM ⛔**

**WARNING: Skipping Phase 6 after Phase 5 will cause serious problems:**
- **No Integration**: Phase 5 results (existingTasks, taskProgress) will not be integrated into new tasks
- **No Deduplication**: Duplicate tasks will remain undetected, causing confusion and wasted effort
- **Incomplete Analysis**: Task breakdown will lack context from existing TODO.md file
- **Data Loss**: Progress status and existing task information will be ignored

**You MUST utilize Phase 5 results (existingTasks, taskProgress) as mandatory input for Phase 6 analysis.**

**Phase 5 Results Reception Confirmation:**
```
Received from Phase 5:
- existingTasks: {N} tasks
- taskProgress: {M} completed, {K} in-progress, {L} pending
- existingQuestions: {P} questions
- duplicateTasks: {Q} duplicate tasks identified
```

3. **Utilizing Phase 1-4 Planning Results**
   - **Referencing Planning Results**
     - Check implementation approach, task breakdown, critical files from `planning_results` variable
     - Reference `docs/memory/planning/YYYY-MM-DD-[feature]-plan.md`
     - Utilize implementation strategy designed by Plan subagent in Phase 3
   - **Existing Research Check**: Check past analysis results in docs/memory to avoid duplicate analysis

4. **Scientific Analysis of Implementation Feasibility**
   - **✅ Ready**: Clear specifications, technical issues clarified, related files confirmed, immediately executable
   - **⏳ Pending**: Waiting for dependencies, specify concrete waiting reasons and release conditions
   - **🔍 Research**: Research required, specify concrete research items and methods
   - **🚧 Blocked**: Important specifications/technical details unclear, specify concrete blocking factors and resolution steps
   - **Verification Basis**: Record files and research results that served as the basis for each determination

5. **Task Breakdown (Minimal Implementation Focus)**
   - **🚨 Most Important Constraint**: Extract only tasks directly necessary to achieve the objective. Do NOT include:
     - Refactoring (improving or organizing existing code)
     - Adding or enhancing logs
     - Adding tests (supplementing tests for existing functions)
     - Strengthening error handling (improving existing functions)
     - Adding or updating documentation
     - Performance optimization
     - Code quality improvement
     - Security strengthening (when not essential for new features)
     - Additional work for pursuing perfection
   - **Required**: Concretely specify the implementation target files for each task
   - Break down complex tasks into implementation units (file units, function units)
   - Determine execution order considering dependencies (specify prerequisites)
   - **Task Granularity Guidelines**:
     - ✅ **One file, one feature per task**: Each task should focus on a single file or feature
     - ✅ **Completable in 30 min - 2 hours**: Tasks should be small enough to complete in one focused session
     - ✅ **Clear dependencies**: Dependencies between tasks must be easily identifiable
     - ❌ **Too broad**: Avoid tasks like "implement XX feature" without specific file/function targets

6. **Building the Strategic Plan (`strategic_plan`)**
   - **Purpose**: Consolidate the feasibility analysis and task breakdown above into the structured output that downstream phases consume. This replaces the standalone strategic-planning step previously delegated to a separate skill call.
   - Produce a `strategic_plan` object with the following fields:
     - `tasks_by_feasibility`: `{ready: [], pending: [], research: [], blocked: []}` — directly reuses the ✅⏳🔍🚧 categorization from step 4
     - `user_questions`: Array of question objects (with 2-4 structured options each) extracted from every item marked 🔍 Research or 🚧 Blocked in step 4, plus any unresolved ambiguity surfaced while breaking down tasks in step 5
     - `checklist_structure`: Complete Markdown checklist (`- [ ]`) combining `detailedTasks` (new tasks from step 5) with `existingTasks`/`taskProgress` (from Phase 5), annotated with feasibility markers (✅⏳🔍🚧) and file references (📁)
     - `implementation_recommendations`: Next actions and quality checkpoints derived from `planning_results.trade_offs` and `planning_results.risks`, validated against the YAGNI constraint in step 5
   - **Note**: `strategic_plan` is NOT saved to disk directly — it is an intermediate structure consumed by Phase 7 (Questions) and Phase 8 (Update), where its contents are persisted into the $ARGUMENTS file and docs/memory

**✅ Phase 6 Completion Confirmation:**
```
✅ Phase 6 Completed:
- Created feasibilityAnalysis with {N} categorized tasks
- Generated detailedTasks: {M} total tasks ({K} existing + {L} new)
- Removed {P} duplicate tasks
- Built strategic_plan: {Q} questions, checklist_structure ready
Proceeding to Phase 7...
```

---

[← Previous: Phase 5](PHASE-6-ANALYSIS.md) | [Main](SKILL.md) | [Next: Phase 7 →](PHASE-8-QUESTIONS.md)
