# CCCP - Claude Code Command Pack

A plugin for Claude Code that provides Git operations specialist agent, Git workflow commands, and task planning and execution workflow (todo-task-planning/todo-task-run).

## Overview

This plugin provides Claude Code with an integrated development support centered on task planning and execution workflow (todo-task-planning/todo-task-run), complemented by Git operation commands:

| Feature | Type | Description | Options |
|---------|------|-------------|---------|
| **git-operations-specialist** | Agent | Expert Git operations including history analysis, conflict resolution, branch strategy, and GitHub CLI operations | - |
| **project-manager** | Agent | Project management and task organization specialist | - |
| **commit** | Command | Commit staged changes with appropriate commit messages | - |
| **micro-commit** | Command | Create fine-grained commits following Lucas Rocha's micro-commit methodology | - |
| **pull-request** | Command | Create or update pull requests for the current branch | `<issue-number>` - Specify issue number |
| **todo-task-planning** | Command | Plan and organize tasks with todo lists | `<filepath>` - TODO file path<br>`--pr` - Create PR<br>`--branch [name]` - Create branch |
| **todo-task-run** | Command | Execute planned tasks from todo lists | `<filepath>` - TODO file path<br>`--no-pr` - Skip PR creation |

## Prerequisites

- [Claude Code](https://claude.com/claude-code) installed
- Git (version 2.0 or higher)
- [GitHub CLI (gh)](https://cli.github.com/) for GitHub operations

## Installation

### From Marketplace (Recommended)

Run the following commands in Claude Code to add the gendosu-claude-plugins marketplace and install the CCCP plugin:

**Step 1: Add gendosu-claude-plugins marketplace**
```bash
/plugin marketplace add gendosu/gendosu-claude-plugins
```

**Step 2: Install CCCP plugin**
```bash
/plugin install cccp@gendosu-claude-plugins
```

Alternatively, use the interactive interface:
```bash
/plugin
```

Search for `cccp` in the `Discover` tab and install it.

### From Source

Clone this repository:
```bash
git clone https://github.com/gendosu/agent-skills.git
```

Then install the plugin in Claude Code:
```bash
/plugin install /path/to/agent-skills
```

## Skills

This plugin provides a unified collection of skills organized by type:

### Agent Skills

Agent skills run in isolated context with specific models for specialized tasks:

#### Git Operations Specialist (`/git-operations-specialist`)

Expert Git operations including:

- **Git History Analysis**: Analyze commit history, branch relationships, and file changes
- **Conflict Resolution**: Guide through merge conflicts with appropriate strategies
- **Branch Strategy**: Recommend and implement branching workflows (GitFlow, GitHub Flow)
- **Advanced Git Operations**: Interactive rebase, cherry-picking, stash management, reflog operations
- **GitHub CLI Operations**: PR creation/management, issue tracking, API operations

#### Project Manager (`/project-manager`)

Project management and task organization specialist.

### Command Skills

Command skills provide workflow operations and development automation:

#### Commit Command (`/commit`)
- Commit staged changes with appropriate commit messages
- Follows conventional commit format and project guidelines

#### Micro-Commit Command (`/micro-commit`)
- Create fine-grained commits following test-driven development cycles
- Group related changes logically
- Maintain clean and meaningful commit history
- Follow one change per commit principle

#### Pull Request Command (`/pull-request`)
- Create new pull requests for the current branch
- Update existing pull requests with latest changes
- Link pull requests to GitHub issues
- Automatically generate PR titles and descriptions

#### Todo Task Workflow

This plugin provides a two-phase workflow for task management:

**Phase 1: Planning (`/todo-task-planning`)**
- Analyze requirements and convert them into actionable tasks
- Create structured TODO.md with checkbox-based task lists
- Use TDD methodology to break down complex requirements
- Define clear task dependencies and priorities

**Phase 2: Execution (`/todo-task-run`)**
- Execute tasks from pre-created TODO.md file
- Manage git operations (branch creation, commits, pushes)
- Create and update pull requests with task progress
- Track completion by updating checkbox status

**Workflow Diagram:**
```
Requirements → /todo-task-planning → TODO.md → /todo-task-run → Pull Request
```

**Implementation Example:**

**Step 1: Create TODO.md** - Write down what you want to do
```markdown
Change email field name to account on login page
```

**Step 2: Run task planning**
```bash
/todo-task-planning TODO.md
```
This command analyzes the requirements and automatically generates an executable task list.

**Step 3: Run task execution**
```bash
/todo-task-run TODO.md
```
This command executes the generated tasks.

### Template Skills

Template skills provide reference materials and standard formats:

#### Key Guidelines (`/key-guidelines`)
- Core development guidelines and best practices
- Reference material for consistent development standards

#### Todo Output Template (`/todo-output-template`)
- Standard TODO.md format specification
- Ensures consistent task planning structure

## Usage

All skills are invoked using the slash command syntax:

### Agent Skills

Agent skills can be invoked explicitly or automatically based on context:

```
/git-operations-specialist        # Explicitly invoke Git operations specialist
/project-manager                  # Explicitly invoke project manager

# Agent skills are also automatically invoked based on context:
"Analyze the git history for this feature branch"
"Help me resolve this merge conflict"
"Suggest a branching strategy for this project"
```

### Command Skills

Invoke command skills directly with optional arguments:

```
/commit                           # Commit staged changes
/micro-commit                     # Create fine-grained commits
/pull-request                     # Create or update pull request
/pull-request 123                 # Create PR linked to issue #123

# Two-phase task workflow:
/todo-task-planning TODO.md       # Phase 1: Plan and create TODO.md
/todo-task-run TODO.md            # Phase 2: Execute tasks from TODO.md
/todo-task-run TODO.md --no-pr    # Execute without creating PR
```

### Template Skills

Template skills provide reference information:

```
/key-guidelines                   # View core development guidelines
/todo-output-template             # View TODO.md format specification
```

## Documentation

For detailed information about the TODO Task workflow, see:

- **[TODO Task Development Guide JP](docs/todo-task-development-guide.jp.md)** - Comprehensive guide covering:
  - Basic concepts and Feasibility Markers
  - `todo-task-planning` command details
  - `todo-task-run` command details
  - Practical workflows and best practices
  - Common mistakes and troubleshooting

## Project Structure

```
cccp/
├── .claude-plugin/
│   └── plugin.json                    # Plugin configuration
├── skills/                            # All skills (agents, commands, templates)
│   ├── git-operations-specialist/     # Git operations skill
│   ├── project-manager/               # Project management agent skill
│   ├── commit/                        # Commit command skill
│   ├── micro-commit/                  # Micro-commit command skill
│   ├── pull-request/                  # Pull request command skill
│   ├── todo-task-planning/            # Task planning command skill
│   ├── todo-task-run/                 # Task execution command skill
│   ├── key-guidelines/                # Core guidelines template skill
│   └── todo-output-template/          # TODO output format template skill
├── docs/
│   └── todo-task-development-guide.md # Comprehensive TODO Task guide
├── LICENSE                            # MIT License
└── README.md                          # This file
```

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Based on Git best practices and workflows
- Implements Lucas Rocha's micro-commit methodology
- Designed for Claude Code plugin ecosystem
