---
name: micro-commit
description: Use when you have uncommitted changes spanning multiple contexts and need to split them into logical, independently meaningful commits — especially before a PR or code review
user-invocable: true
---

## Overview

Splits uncommitted changes into small, logical commits — one per feature, fix, or layer. Each commit should be independently meaningful and reviewable.

## When to Use

- Working tree has changes across multiple files with different purposes
- About to open a PR and want a clean, readable history
- Mixed changes: feature + refactor + config in one dirty working tree

**Not needed when:** all changes belong to a single logical unit — just commit normally.

## Instructions

**IMPORTANT: ALL git operations (status checks, staging, committing) MUST be delegated to a Haiku sub-agent via the `Agent` tool. Do NOT execute any git commands directly in the main session.**

Execute the following steps:

### Step 1: Launch a Haiku sub-agent

Use the `Agent` tool with `model: "haiku"` and pass the following prompt verbatim:

````
You are a Git Operations Specialist. Your job is to collect the current repository state, group changes into logical units, and execute micro-commits.

## Step A: Collect Repository State

Run the following commands using the Bash tool:

```bash
git status --short
git diff HEAD
git ls-files --others --exclude-standard
```

For each untracked file listed by `git ls-files`, read its contents using the Read tool.

## Step B: Group Changes

Group changes into logical commits using these criteria (in order of preference):
- By feature: files that implement the same feature
- By layer: API / model / frontend / config / test / docs
- By purpose: new feature, bug fix, refactoring, configuration

## Step C: Execute Micro-Commits

For each logical group:
1. Stage files explicitly — use `git add <file>` for tracked changes, `git add <untracked-file>` for new files
2. Commit with a clear message using this exact HEREDOC format:
```
git commit -m "$(cat <<'EOF'
<type>(<scope>): <description>
EOF
)"
```
3. Run `git status --short` after each commit to verify it succeeded before proceeding

Commit type prefixes: feat, fix, refactor, docs, style, test, chore
- One logical change per commit
- Process groups sequentially
- If a commit fails (e.g., pre-commit hook error), report the error and stop — do not force-skip hooks

After all commits, run `git status` to confirm the working tree is clean.

## Required Return Format

Return a summary report with:
- List of commits created: git hash + message + files included
- Any errors encountered and which files were skipped
- Final repository status (branch name, commits ahead of remote, working tree state)
````

### Step 2: Report results

Relay the sub-agent's summary report to the user. If the sub-agent reported errors, surface them clearly so the user can take action.

## Common Mistakes

| Mistake | Fix |
|---|---|
| HEREDOC `EOF` is indented | `EOF` must be at column 0; use `<<-'EOF'` only if stripping tabs intentionally |
| Staging all files at once (`git add .`) | Stage files per logical group to keep commits focused |
| Skipping `git status` between commits | Always verify each commit succeeded before proceeding |
| Force-skipping hooks (`--no-verify`) | Fix the hook error instead — don't bypass it |
