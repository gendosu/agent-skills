# CCCP - Claude Code Command Pack

A plugin for Claude Code that provides Git operations specialist, Git workflow commands, and task planning and execution workflow (todo-task-planning/todo-task-run).

## Overview

This plugin provides Claude Code with an integrated development support centered on task planning and execution workflow (todo-task-planning/todo-task-run), complemented by Git operation commands:

| Feature | Description | Options |
|---------|-------------|---------|
| **git-operations-specialist** | Expert Git operations including history analysis, conflict resolution, branch strategy, and GitHub CLI operations | - |
| **project-manager** | Project management and task organization specialist | - |
| **commit** | Commit staged changes with appropriate commit messages | - |
| **micro-commit** | Create fine-grained commits following Lucas Rocha's micro-commit methodology | - |
| **pull-request** | Create or update pull requests for the current branch | `<issue-number>` - Specify issue number |
| **todo-task-planning** | Plan and organize tasks with todo lists | `<filepath>` - TODO file path<br>`--pr` - Create PR<br>`--branch [name]` - Create branch |
| **todo-task-run** | Execute planned tasks from todo lists | `<filepath>` - TODO file path |

## Prerequisites

- One of the following AI agents installed:
  - [Claude Code](https://claude.com/claude-code)
  - [OpenAI Codex](https://openai.com/index/openai-codex/)
  - [Cursor](https://www.cursor.com/)
- Git (version 2.0 or higher)
- [GitHub CLI (gh)](https://cli.github.com/) for GitHub operations

## Installation

### Supported AI Agents

This skill repository can be used with the following AI agents:

| Agent | Type | Installation |
|-------|------|-------------|
| **Claude Code** | Anthropic's official CLI | Plugin system |
| **OpenAI Codex** | OpenAI's agent system | Agent skills directory |
| **Cursor** | AI pair programming editor | Agent skills directory (same as Codex) |

### Installation by Agent

#### Claude Code

**Storage Location**: Any directory

**Installation Methods**:

**Option 1: From Marketplace (Recommended)**

```bash
# Step 1: Add marketplace
/plugin marketplace add gendosu/gendosu-claude-plugins

# Step 2: Install plugin
/plugin install cccp@gendosu-claude-plugins
```

Alternatively, use the interactive interface:
```bash
/plugin
```

Search for `cccp` in the `Discover` tab and install it.

**Option 2: From Source**

```bash
# Clone repository
git clone https://github.com/gendosu/agent-skills.git

# Install plugin
/plugin install /path/to/agent-skills
```

---

#### OpenAI Codex / Cursor

**Storage Location**:
- **Recommended**: `~/.agents/skills/cccp` (user-wide)
- **Alternative**: `.agents/skills` (project-specific) or any directory (with skill installer)

**Prerequisites**:
- OpenAI Codex or Cursor installed
- Basic understanding of agent skills system

**Installation Methods**:

**Option 1: Using Skill Installer (Recommended)**

1. Invoke the skill installer:
   ```
   $skill-installer
   ```

2. Clone repository to your preferred location:
   ```bash
   git clone https://github.com/gendosu/agent-skills.git
   ```

3. Install individual skills from the repository:
   ```
   install the <skill-name> skill from /path/to/agent-skills/skills/<skill-name>
   ```

**Option 2: Manual Installation**

```bash
# Clone to recommended location
git clone https://github.com/gendosu/agent-skills.git ~/.agents/skills/cccp
```

Each skill directory in `skills/` contains a `SKILL.md` file that will be automatically detected.

Restart Codex/Cursor to load the newly installed skills.

**Skill Search Paths** (in order of precedence):
- **Project-specific**: `.agents/skills` (repository root)
- **User-wide**: `~/.agents/skills` (recommended)
- **System-wide**: `/etc/codex/skills` (organization defaults)

#### Available Skills for Codex/Cursor

The following skills are compatible with OpenAI Codex and Cursor:

- **git-operations-specialist**: Git operations expert (history, conflicts, branching)
- **project-manager**: Project management and task organization
- **commit**: Commit staged changes with conventional commit messages
- **micro-commit**: Fine-grained commits following TDD methodology
- **pull-request**: Create and update pull requests
- **todo-task-planning**: Plan and organize tasks with TODO lists
- **todo-task-run**: Execute planned tasks from TODO files
- **key-guidelines**: Core development guidelines reference
- **todo-output-template**: Standard TODO.md format specification

#### Using Skills in Codex/Cursor

**Explicit Invocation:**
```
$git-operations-specialist
$todo-task-planning
```

**Implicit Invocation:**
Codex and Cursor automatically select appropriate skills based on your task description.

For more information about agent skills, visit the [OpenAI Codex Skills Documentation](https://developers.openai.com/codex/skills/).

## Skills

This plugin provides a unified collection of skills:

### Git Operations Specialist (`/git-operations-specialist`)

Expert Git operations including:

- **Git History Analysis**: Analyze commit history, branch relationships, and file changes
- **Conflict Resolution**: Guide through merge conflicts with appropriate strategies
- **Branch Strategy**: Recommend and implement branching workflows (GitFlow, GitHub Flow)
- **Advanced Git Operations**: Interactive rebase, cherry-picking, stash management, reflog operations
- **GitHub CLI Operations**: PR creation/management, issue tracking, API operations

### Project Manager (`/project-manager`)

Project management and task organization specialist.

### Commit Command (`/commit`)
- Commit staged changes with appropriate commit messages
- Follows conventional commit format and project guidelines

### Micro-Commit Command (`/micro-commit`)
- Create fine-grained commits following test-driven development cycles
- Group related changes logically
- Maintain clean and meaningful commit history
- Follow one change per commit principle

### Pull Request Command (`/pull-request`)
- Create new pull requests for the current branch
- Update existing pull requests with latest changes
- Link pull requests to GitHub issues
- Automatically generate PR titles and descriptions

### Todo Task Workflow

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

### Key Guidelines (`/key-guidelines`)
- Core development guidelines and best practices
- Reference material for consistent development standards

### Todo Output Template (`/todo-output-template`)
- Standard TODO.md format specification
- Ensures consistent task planning structure

## Usage

All skills are invoked using the slash command syntax:

```
# Git and project management
/git-operations-specialist        # Invoke Git operations specialist
/project-manager                  # Invoke project manager

# Git workflow commands
/commit                           # Commit staged changes
/micro-commit                     # Create fine-grained commits
/pull-request                     # Create or update pull request
/pull-request 123                 # Create PR linked to issue #123

# Two-phase task workflow
/todo-task-planning TODO.md       # Phase 1: Plan and create TODO.md
/todo-task-run TODO.md            # Phase 2: Execute tasks from TODO.md

# Reference materials
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
