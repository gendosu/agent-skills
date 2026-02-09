# Phase 0: Preparation

[← Main](SKILL.md) | [Next: Phase 1 →](PHASE-1-ANALYSIS.md)

---

### Phase 0: Multi-Subagent Orchestration (Main Claude Executor)

---

## [CRITICAL] CRITICAL EXECUTION RULE [CRITICAL]

**[PROHIBITED] ABSOLUTE PROHIBITION: NEVER CALL MULTIPLE TASK TOOLS IN A SINGLE MESSAGE [PROHIBITED]**

**YOU MUST:**
- [OK]Call **ONE Task tool per message/response**
- [OK]Wait for the Task tool result to arrive
- [OK]Verify the result contains expected data
- [OK]**THEN** proceed to the next phase in a **NEW message**

**YOU MUST NOT:**
- [NG]Call multiple Task tools in the same `<function_calls>` block
- [NG]Proceed to the next phase without receiving and verifying the previous result
- [NG]Assume parallel execution will work correctly

**Why This Matters:**
- When multiple Task tools are called in a single message, Claude Code executes them **IN PARALLEL**
- Phase 0.3 (Plan) **REQUIRES** `exploration_results` from Phase 0.2 (Explore)
- Phase 0.4 (project-manager) **REQUIRES** both `exploration_results` AND `planning_results`
- Parallel execution causes **data dependency failures** and **incomplete planning**

**Execution Flow:**
```
Message 1: Call Task (Explore) → WAIT for result
    ↓
[Receive exploration_results]
    ↓
Message 2: Call Task (Plan) with exploration_results → WAIT for result
    ↓
[Receive planning_results]
    ↓
Message 3: Call Task (project-manager) with both results → WAIT for result
    ↓
[Receive strategic_plan]
```

---

**[WARNING]CRITICAL: Sequential Execution Required**

The subagents in Phase 0 MUST be executed in the following order:
1. Phase 0.1: TODO File Reading
2. Phase 0.2: Explore Subagent
3. Phase 0.3: Plan Subagent
4. Phase 0.4: project-manager skill
5. Phase 0.5: Verification

**DO NOT execute subagents in parallel.** Each phase depends on the results of the previous phase:
- Phase 0.3 (Plan) requires `exploration_results` from Phase 0.2 (Explore)
- Phase 0.4 (project-manager skill) requires both `exploration_results` and `planning_results`

**Execution Pattern:**
```typescript
// ✅ CORRECT: ONE Task tool call per message
// Message 1:
Task({ subagent_type: "Explore", ... });
// → WAIT for result, receive exploration_results

// Message 2 (AFTER receiving exploration_results):
Task({
  subagent_type: "Plan",
  prompt: `... ${exploration_results.summary} ...`
});
// → WAIT for result, receive planning_results

// Message 3 (AFTER receiving planning_results):
Task({
  subagent_type: "project-manager",
  prompt: `... ${exploration_results.summary} ... ${planning_results.approach_summary} ...`
});

// ❌ WRONG: Multiple Task tools in ONE message (causes parallel execution)
// Single message with multiple Task calls:
Task({ subagent_type: "Explore", ... });
Task({ subagent_type: "Plan", ... });  // [PROHIBITED]This will run IN PARALLEL, breaking dependencies!
Task({ subagent_type: "project-manager", ... });  // [PROHIBITED]This will run IN PARALLEL!

// ❌ WRONG: Parallel execution pattern
Promise.all([
  Task({ subagent_type: "Explore", ... }),
  Task({ subagent_type: "Plan", ... })  // Plan cannot start before Explore completes!
]);
```

#### Phase 0.1: TODO File Reading and Context Extraction

1. **Reading $ARGUMENTS File**
   - Read the specified file with Read tool
   - Extract information on tasks, requirements, and tech stack
   - Determine exploration thoroughness (quick/medium/very thorough)

2. **Git Workflow Options Preparation**

   Parse the `$ARGUMENTS` string to extract file path and command-line flags, then prepare variables for subsequent phase execution.

   **Argument Parsing Procedure**:

   **a) Parse `$ARGUMENTS` string**:
   - Split `$ARGUMENTS` on whitespace to get an array of tokens
   - First element is the file path (e.g., `TODO.md`, `docs/todos/feature-x.md`)
   - Remaining elements are command-line flags (e.g., `--pr`, `--branch`, `feature/auth`)

   **b) Detect `--pr` flag**:
   ```
   IF `$ARGUMENTS` contains `--pr` THEN
       → Set `HAS_PR_OPTION = true`
   ELSE
       → Set `HAS_PR_OPTION = false`
   END IF
   ```

   **c) Detect `--branch` flag and extract value**:
   ```
   IF `$ARGUMENTS` contains `--branch` THEN
       → Set `HAS_BRANCH_OPTION = true`
       → Find the token immediately after `--branch`
       IF next token exists AND does NOT start with `--` THEN
           → Set `BRANCH_NAME = [token value]`
           → Set `IS_AUTO_GENERATED = false`
       ELSE
           → Set `BRANCH_NAME = ""` (empty string, will be generated in next step)
           → Set `IS_AUTO_GENERATED = true`
       END IF
   ELSE
       → Set `HAS_BRANCH_OPTION = false`
       → Set `BRANCH_NAME = ""` (empty string)
       → Set `IS_AUTO_GENERATED = false`
   END IF
   ```

   **d) Validation logic - PR requires branch**:
   ```
   IF `HAS_PR_OPTION = true` AND `HAS_BRANCH_OPTION = false` THEN
       → Automatically set `HAS_BRANCH_OPTION = true`
       → Set `IS_AUTO_GENERATED = true`
       → Set `BRANCH_NAME = ""` (will be auto-generated in next step)
       → Rationale: Pull requests require a branch, so enable branch creation automatically
   END IF
   ```

   **e) Variable summary**:

   | Variable | Type | Purpose | Set By |
   |----------|------|---------|--------|
   | `HAS_BRANCH_OPTION` | boolean | Whether branch creation is needed | `--branch` flag or PR validation |
   | `HAS_PR_OPTION` | boolean | Whether PR creation is needed | `--pr` flag |
   | `BRANCH_NAME` | string | Branch name (empty if auto-generated) | Explicit `--branch <name>` argument |
   | `IS_AUTO_GENERATED` | boolean | Whether branch name needs generation | Flag without value or PR validation |

   **Parsing Examples**:

   **Example 1: Both flags with explicit branch name**
   ```
   Input: /todo-task-planning TODO.md --pr --branch feature/auth
   Result:
   - HAS_PR_OPTION = true
   - HAS_BRANCH_OPTION = true
   - BRANCH_NAME = "feature/auth"
   - IS_AUTO_GENERATED = false
   ```

   **Example 2: Branch flag without value**
   ```
   Input: /todo-task-planning docs/todos/feature-x.md --branch
   Result:
   - HAS_PR_OPTION = false
   - HAS_BRANCH_OPTION = true
   - BRANCH_NAME = "" (empty, will be generated)
   - IS_AUTO_GENERATED = true
   ```

   **Example 3: PR flag auto-enables branch creation**
   ```
   Input: /todo-task-planning TODO.md --pr
   Result:
   - HAS_PR_OPTION = true
   - HAS_BRANCH_OPTION = true (auto-enabled by validation)
   - BRANCH_NAME = "" (empty, will be generated)
   - IS_AUTO_GENERATED = true (set by validation)
   ```

   **Example 4: No flags**
   ```
   Input: /todo-task-planning docs/feature.md
   Result:
   - HAS_PR_OPTION = false
   - HAS_BRANCH_OPTION = false
   - BRANCH_NAME = "" (empty)
   - IS_AUTO_GENERATED = false
   ```

3. **Branch Name Generation (if --branch option specified without value)**
   - Read TODO file title and overview
   - Determine branch type:
     - `feature/` - New functionality (default for most implementations)
     - `bugfix/` - Bug fixes mentioned in TODO
     - `refactor/` - Code restructuring mentioned
     - `chore/` - Tooling, dependencies, environment setup
     - `docs/` - Documentation-focused tasks
   - Extract key feature name (2-4 words)
   - Convert to lowercase, hyphen-separated English
   - Validate against Git naming conventions
   - Set generated branch name to `BRANCH_NAME` variable and set `IS_AUTO_GENERATED = true`
   - Example: "ActionLog通知実装" → `feature/actionlog-notification`

4. **Context Preparation for Exploration**
   - Identify feature areas and related keywords
   - Determine exploration scope (file patterns, directories)
   - Check existing docs/memory research results (to avoid duplicate research)

#### Phase 0.2: Calling Explore Subagent

**Purpose**: Discover related files, patterns, and dependencies through comprehensive codebase exploration

**[CRITICAL]EXECUTION REQUIREMENT: Call Task tool in THIS message, THEN WAIT for result**

**YOU MUST:**
1. Call the Explore Task tool in this message
2. **STOP** after calling the Task tool
3. **WAIT** for the tool result to arrive
4. Verify `exploration_results` contains valid data
5. **DO NOT** call Plan or project-manager Task tools in the same message

**Task tool execution example**:
```typescript
// Conceptual example - Explore subagent for codebase investigation
const exploration_result = await Task({
  subagent_type: "Explore",
  description: "Codebase exploration for [feature name]",
  prompt: `
    Investigate [feature area] in the codebase.

    Focus on: related files, dependencies, test coverage, existing patterns.
    Thoroughness: [quick/medium/very thorough]

    Return: key files, patterns, tech stack, blockers, recommendations.
  `
});
```

**Saving Results**:
- **Subagent responsibility**:
  - Return structured data in variable `exploration_results` containing:
    - `summary`: Overall findings summary (required)
    - `files`: Array of {path, purpose, importance} objects
    - `patterns`: Existing patterns and conventions
    - `tech_stack`: Technologies and frameworks used
    - `blockers`: Potential blockers and constraints
    - `recommendations`: Recommendations for planning phase
  - **IMPORTANT**: Subagent does NOT create files directly (Task tool limitation)

- **Main Claude executor responsibility** (executed in Phase 4):
  - **MANDATORY**: Use Write tool to create `docs/memory/explorations/YYYY-MM-DD-[feature]-exploration.md`
  - Format: Transform exploration_results data into structured markdown
  - Sections: Summary, Key Discoveries, Patterns, Tech Stack, Blockers, Recommendations
  - Verification: File creation will be confirmed in Phase 0.5

**[WARNING]WAIT: Verify Explore Subagent Completion**

**THIS IS A MANDATORY CHECKPOINT - DO NOT PROCEED UNTIL VERIFIED:**

Before proceeding to Phase 0.3, ensure:
- [ ] Explore subagent Task tool has completed successfully
- [ ] You have **RECEIVED** the Task tool result in the conversation
- [ ] `exploration_results` variable contains valid data
- [ ] `docs/memory/explorations/` file has been created
- [ ] NO errors occurred during exploration

**IF ANY OF THE ABOVE ARE NOT TRUE:**
- [PROHIBITED]**STOP** - Do not proceed to Phase 0.3
-Investigate what went wrong
-Fix the issue before continuing

**ONLY after confirming ALL of the above in a NEW message, proceed to Phase 0.3 (Plan Subagent).**

#### Phase 0.3: Calling Plan Subagent

**Purpose**: Design implementation strategy based on exploration results

**[CRITICAL]EXECUTION REQUIREMENT: Call Task tool in THIS message, THEN WAIT for result**

**YOU MUST:**
1. Verify `exploration_results` exists from Phase 0.2
2. Call the Plan Task tool in this message (pass `exploration_results` in prompt)
3. **STOP** after calling the Task tool
4. **WAIT** for the tool result to arrive
5. Verify `planning_results` contains valid data
6. **DO NOT** call project-manager Task tool in the same message

**[WARNING]MANDATORY PRECONDITION: Phase 0.2 Explore Subagent MUST Be Completed First**

**DO NOT proceed with Phase 0.3 unless ALL of the following are confirmed:**
- [OK]Phase 0.2 Explore subagent has successfully completed
- [OK]`exploration_results` variable exists and contains data
- [OK]Exploration file saved: `docs/memory/explorations/YYYY-MM-DD-[feature]-exploration.md`
- [OK]No errors occurred during exploration

**Verify Argument Parsing Variables (from Phase 0.1 Step 2)**:
- [OK]`HAS_PR_OPTION` is set (boolean value, not undefined)
- [OK]`HAS_BRANCH_OPTION` is set (boolean value, not undefined)
- [OK]`BRANCH_NAME` is set (string value, may be empty if auto-generation needed)
- [OK]`IS_AUTO_GENERATED` is set (boolean value, not undefined)
- [OK]If `HAS_BRANCH_OPTION = true` and `IS_AUTO_GENERATED = true`, verify `BRANCH_NAME` was populated in Phase 0.1 Step 3
- [OK]Report to user if variables are NOT set (CRITICAL ERROR)

**Why This Matters:**
The Plan subagent requires exploration results (`exploration_results.summary`, `exploration_results.files`, `exploration_results.patterns`, `exploration_results.tech_stack`) to create an accurate implementation plan. Running Plan before Explore completes will result in incomplete or incorrect planning.

**ONLY after confirming the above, execute the Plan subagent Task tool.**

**Task tool execution example**:
```typescript
// ⚠️ IMPORTANT: This Task call MUST happen AFTER Phase 0.2 (Explore) completes
// Verify exploration_results exists before proceeding
if (!exploration_results) {
  throw new Error("Cannot proceed: exploration_results not found. Phase 0.2 must complete first.");
}

Task({
  subagent_type: "Plan",
  description: "Implementation planning for [feature name]",
  prompt: `
    Create detailed implementation plan using exploration results.
    Include: approach, step-by-step tasks, critical files, trade-offs, risks, feasibility assessment.
    Context: ${exploration_results.summary}
  `
})
```

**Saving Results**:
- **Subagent responsibility**:
  - Return structured data in variable `planning_results` containing:
    - `approach_summary`: Implementation strategy (2-3 paragraphs)
    - `tasks`: Array of task objects with descriptions and dependencies
    - `critical_files`: Files to create/modify with their roles
    - `trade_offs`: Technical trade-offs analysis
    - `risks`: Potential risks and mitigation strategies
    - `feasibility`: Task categorization by status (✅⏳🔍🚧)
  - **IMPORTANT**: Subagent does NOT create files directly (Task tool limitation)

- **Main Claude executor responsibility** (executed in Phase 4):
  - **MANDATORY**: Use Write tool to create `docs/memory/planning/YYYY-MM-DD-[feature]-plan.md`
  - Format: Transform planning_results data into structured markdown
  - Sections: Approach, Task Breakdown, Critical Files, Trade-offs, Risks and Mitigation, Feasibility Assessment
  - Verification: File creation will be confirmed in Phase 0.5

**[WARNING]WAIT: Verify Plan Subagent Completion**

**THIS IS A MANDATORY CHECKPOINT - DO NOT PROCEED UNTIL VERIFIED:**

Before proceeding to Phase 0.4, ensure:
- [ ] Plan subagent Task tool has completed successfully
- [ ] You have **RECEIVED** the Task tool result in the conversation
- [ ] `planning_results` variable contains valid data
- [ ] `planning_results` includes exploration context from Phase 0.2
- [ ] `docs/memory/planning/` file has been created
- [ ] NO errors occurred during planning

**IF ANY OF THE ABOVE ARE NOT TRUE:**
- [PROHIBITED]**STOP** - Do not proceed to Phase 0.4
-Investigate what went wrong
-Fix the issue before continuing

**ONLY after confirming ALL of the above in a NEW message, proceed to Phase 0.4 (project-manager).**

#### Phase 0.4: Calling project-manager Skill

**Purpose**: Integrate exploration and planning results and organize strategically

**[CRITICAL]EXECUTION REQUIREMENT: Call Task tool in THIS message, THEN WAIT for result**

**YOU MUST:**
1. Verify both `exploration_results` and `planning_results` exist
2. Call the project-manager Task tool in this message (pass both results in prompt)
3. **STOP** after calling the Task tool
4. **WAIT** for the tool result to arrive
5. Verify `strategic_plan` contains valid data
6. Proceed to Phase 0.5 verification

**[WARNING]MANDATORY PRECONDITION: Both Phase 0.2 AND Phase 0.3 MUST Be Completed First**

**DO NOT proceed with Phase 0.4 unless ALL of the following are confirmed:**
- [OK]Phase 0.2 Explore subagent has successfully completed
- [OK]Phase 0.3 Plan subagent has successfully completed
- [OK]Both `exploration_results` and `planning_results` variables exist
- [OK]Both exploration and planning files exist in `docs/memory/`
- [OK]No errors occurred during exploration or planning

**Sequential Dependency Chain:**
```
Phase 0.2 (Explore) → exploration_results
                          ↓
Phase 0.3 (Plan) → planning_results
                          ↓
Phase 0.4 (project-manager) → strategic_plan
```

**ONLY after confirming the above, execute the project-manager skill Task tool.**

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

**Saving Results**:
- **Subagent responsibility**:
  - Return structured data in variable `strategic_plan` containing:
    - `tasks_by_feasibility`: {ready: [], pending: [], research: [], blocked: []}
    - `user_questions`: Array of question objects with options (for AskUserQuestion tool)
    - `checklist_structure`: Complete markdown checklist format
    - `implementation_recommendations`: Next action items and quality metrics

- **Note**: strategic_plan is NOT saved to disk
  - **Reason**: Used as intermediate data structure for organizing tasks in Phase 4
  - **Persistence**: Strategic plan data (tasks, questions, checklist) are integrated into $ARGUMENTS file in Phase 4
  - **Reference**: Exploration and planning files already provide persistent storage of analysis results

#### Phase 0.5: Result Verification and Preparation

1. **Subagent Execution Verification**
   - **[WARNING]Verify Sequential Execution Order**
     - [ ] Phase 0.2 (Explore) completed FIRST
     - [ ] Phase 0.3 (Plan) completed SECOND (after Explore)
     - [ ] Phase 0.4 (project-manager) completed THIRD (after Plan)
   - **Confirm all subagents completed successfully**
     - [ ] No errors in Explore subagent execution
     - [ ] No errors in Plan subagent execution
     - [ ] No errors in project-manager skill execution
   - **Verify Variable Dependencies**
     - [ ] `exploration_results` exists and contains valid data
     - [ ] `planning_results` exists and contains valid data
     - [ ] `strategic_plan` exists and contains valid data
   - **Report to user if there are errors**
     - Clearly state which phase failed
     - Explain the impact on subsequent phases
     - Recommend corrective action

2. **docs/memory File Confirmation**
   - [ ] **Check if exploration file exists**: `docs/memory/explorations/YYYY-MM-DD-[feature]-exploration.md`
     - **If NOT exists**: [CRITICAL]CRITICAL ERROR - Exploration file must be created
       - Action: Create file immediately using Write tool with exploration_results data
       - Format: Structured markdown with Summary, Files, Patterns, Tech Stack, Blockers, Recommendations sections
       - Verify exploration_results variable has valid data before creating file
   - [ ] **Check if planning file exists**: `docs/memory/planning/YYYY-MM-DD-[feature]-plan.md`
     - **If NOT exists**: [CRITICAL]CRITICAL ERROR - Planning file must be created
       - Action: Create file immediately using Write tool with planning_results data
       - Format: Structured markdown with Approach, Tasks, Critical Files, Trade-offs, Risks, Feasibility sections
       - Verify planning_results variable has valid data before creating file
   - [ ] **Verify file contents are complete**
     - Exploration file: Must contain summary, files list, patterns, tech_stack, blockers, recommendations
     - Planning file: Must contain approach, tasks, critical_files, trade_offs, risks, feasibility
   - [ ] **Report to user if files were NOT created in Phase 4**
     - State which files are missing and why
     - Confirm files have been created as recovery action

3. **Preparation for Next Phase**
   - If `strategic_plan.user_questions` exists, use in Phase 3
   - Use `strategic_plan.checklist_structure` in Phase 4
   - Retain `exploration_results` and `planning_results` as reference information

4. **Proceed to Phase 1**
   - Execute existing phases using subagent results

---

[← Main](SKILL.md) | [Next: Phase 1 →](PHASE-1-ANALYSIS.md)
