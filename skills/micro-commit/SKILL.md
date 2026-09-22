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

When delegation is available and permitted by the host environment, delegate all Git operations (status checks, staging, committing) to one sub-agent using the host's supported delegation tool. If delegation is unavailable or prohibited, execute the same workflow directly in the main session.

## Model Selection

When the host agent supports model selection for the sub-agent, prefer a low-cost, fast model from those available in that environment, provided it can reliably review diffs, group changes, and execute Git operations. Use the host's documented model identifiers and selection mechanism; do not assume a particular provider or invent model names or parameters. Respect explicit user and project model settings. If model selection is unavailable or no suitable alternative is known, omit the model override and use the host's default or inherited model. For direct execution, keep the current model.

Execute the following steps:

### Step 1: Run the Git operations workflow

Use the host's delegation tool with the model selected above and pass the following prompt. If delegation is unavailable or prohibited, follow the prompt directly:

````
You are a Git Operations Specialist. Your job is to collect the current repository state, group changes into logical units, and execute micro-commits.

## Step A: Collect Repository State

Run the following commands:

```bash
git status --short
git diff HEAD
git ls-files --others --exclude-standard
```

Read the contents of each untracked file listed by `git ls-files`.

## Step B: Group Changes

Group changes into logical commits using these criteria (in order of preference):
- By feature: files that implement the same feature
- By layer: API / model / frontend / config / test / docs
- By purpose: new feature, bug fix, refactoring, configuration

## Step C: Execute Micro-Commits

For each logical group:
1. Stage files explicitly — use `git add <file>` for tracked changes, `git add <untracked-file>` for new files. Never use `git add .` or `git add -A`: staging everything at once destroys the per-group boundaries
2. Commit with a clear message in this format:
```bash
git commit -m "<type>(<scope>): <description>"
```

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

Relay the summary report to the user, whether the workflow ran in a sub-agent or the main session. Surface any errors clearly so the user can take action.
