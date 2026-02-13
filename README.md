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

### For OpenAI Codex

This repository can also be used as agent skills for OpenAI Codex. Agent skills extend Codex with specialized capabilities by packaging instructions, resources, and optional scripts.

#### Prerequisites

- OpenAI Codex installed
- Basic understanding of Codex agent skills system

#### Installation Methods

**Option 1: Using Skill Installer (Recommended)**

1. Invoke the skill installer in Codex:
   ```
   $skill-installer
   ```

2. Clone this repository to your preferred location:
   ```bash
   git clone https://github.com/gendosu/agent-skills.git
   ```

3. Install individual skills from the repository:
   ```
   install the <skill-name> skill from /path/to/agent-skills/skills/<skill-name>
   ```

**Option 2: Manual Installation**

1. Clone this repository:
   ```bash
   git clone https://github.com/gendosu/agent-skills.git ~/.agents/skills/cccp
   ```

2. Each skill directory in `skills/` contains a `SKILL.md` file that Codex will automatically detect.

3. Restart Codex to load the newly installed skills.

#### Skill Locations

Codex searches for skills in the following directories (in order of precedence):
- **Project-specific**: `.agents/skills` (repository root)
- **User-wide**: `~/.agents/skills` (recommended for CCCP installation)
- **System-wide**: `/etc/codex/skills` (organization defaults)

#### Available Skills for Codex

The following skills are compatible with OpenAI Codex:

- **git-operations-specialist**: Git operations expert (history, conflicts, branching)
- **project-manager**: Project management and task organization
- **commit**: Commit staged changes with conventional commit messages
- **micro-commit**: Fine-grained commits following TDD methodology
- **pull-request**: Create and update pull requests
- **todo-task-planning**: Plan and organize tasks with TODO lists
- **todo-task-run**: Execute planned tasks from TODO files
- **key-guidelines**: Core development guidelines reference
- **todo-output-template**: Standard TODO.md format specification

#### Using Skills in Codex

**Explicit Invocation:**
```
$git-operations-specialist
$todo-task-planning
```

**Implicit Invocation:**
Codex automatically selects appropriate skills based on your task description.

For more information about Codex agent skills, visit the [OpenAI Codex Skills Documentation](https://developers.openai.com/codex/skills/).

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
