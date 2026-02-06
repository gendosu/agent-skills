# Contributing to CCCP

Thank you for your interest in contributing to CCCP (Claude Code Command Pack)! This document provides guidelines and instructions for contributing to the project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Process](#development-process)
- [Commit Message Guidelines](#commit-message-guidelines)
- [Pull Request Process](#pull-request-process)
- [Versioning](#versioning)

## Code of Conduct

By participating in this project, you agree to maintain a respectful and collaborative environment for all contributors.

## Getting Started

1. Fork the repository
2. Clone your fork:
   ```bash
   git clone https://github.com/YOUR_USERNAME/agent-skills.git
   cd agent-skills
   ```
3. Add the upstream repository:
   ```bash
   git remote add upstream https://github.com/gendosu/agent-skills.git
   ```

## Development Process

### Setting Up Development Environment

1. Install prerequisites:
   - [Claude Code](https://claude.com/claude-code)
   - Git (version 2.0 or higher)
   - [GitHub CLI (gh)](https://cli.github.com/)

2. Install the plugin locally:
   ```bash
   /plugin install /path/to/agent-skills
   ```

### Making Changes

1. Create a feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. Make your changes following these guidelines:
   - Keep changes focused and atomic
   - Test your changes thoroughly
   - Update documentation as needed
   - Follow the existing code style

3. Test your changes:
   - Test commands in Claude Code
   - Verify agent behaviors
   - Check documentation accuracy

## Commit Message Guidelines

We follow [Conventional Commits](https://www.conventionalcommits.org/) specification:

### Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types

- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only changes
- `style`: Code style changes (formatting, missing semicolons, etc.)
- `refactor`: Code refactoring without changing functionality
- `perf`: Performance improvements
- `test`: Adding or updating tests
- `chore`: Maintenance tasks, dependency updates

### Scope (Optional)

- `agent`: Changes to agent files
- `command`: Changes to command files
- `skill`: Changes to skill files
- `docs`: Documentation changes
- `plugin`: Plugin configuration changes

### Examples

```
feat(command): add new micro-commit command

Implement Lucas Rocha's micro-commit methodology for creating
fine-grained commits following TDD cycles.

Closes #123
```

```
fix(skill): resolve git-operations-specialist parameter handling

Update parameter parsing to handle optional branch names correctly.
```

```
docs: update README with installation instructions

Add marketplace installation steps and improve feature descriptions.
```

## Pull Request Process

1. Update your branch with the latest upstream changes:
   ```bash
   git fetch upstream
   git rebase upstream/main
   ```

2. Push your changes to your fork:
   ```bash
   git push origin feature/your-feature-name
   ```

3. Create a Pull Request:
   - Use the PR template
   - Provide a clear title and description
   - Reference related issues
   - Add appropriate labels

4. Code Review:
   - Address review feedback promptly
   - Keep discussions focused and professional
   - Update your PR based on feedback

5. Merging:
   - PRs require approval before merging
   - Squash commits if necessary
   - Ensure CI passes (if applicable)

### Pull Request Checklist

- [ ] Code follows project conventions
- [ ] Commits follow Conventional Commits format
- [ ] Documentation updated (README, command docs, etc.)
- [ ] Changes tested in Claude Code
- [ ] PR description clearly explains changes
- [ ] Related issues referenced

## Versioning

This project follows [Semantic Versioning](https://semver.org/):

- **MAJOR** version: Incompatible API changes
- **MINOR** version: New functionality (backwards-compatible)
- **PATCH** version: Bug fixes (backwards-compatible)

Version format: `MAJOR.MINOR.PATCH` (e.g., `1.2.3`)

### Version Bump Guidelines

- Breaking changes → MAJOR version bump
- New features → MINOR version bump
- Bug fixes → PATCH version bump

## Project Structure

```
agent-skills/
├── .claude-plugin/
│   └── plugin.json           # Plugin configuration with version
├── agents/                   # Agent definitions
├── commands/                 # Command implementations
├── skills/                   # Reusable skills
├── docs/                     # Documentation
├── CHANGELOG.md             # Version history
└── README.md                # Project overview
```

## Documentation Guidelines

- Keep documentation clear and concise
- Update README.md for user-facing changes
- Update command documentation for command changes
- Maintain CHANGELOG.md with version changes
- Use markdown formatting consistently

## Questions?

If you have questions or need help:

1. Check existing documentation
2. Search existing issues
3. Create a new issue with your question

Thank you for contributing to CCCP!
