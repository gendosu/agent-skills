---
name: key-guidelines
description: |
  Core development principles and standards for consistent, high-quality code.
  Automatically applies DRY, KISS, YAGNI, SOLID, TDD, and micro-commit methodologies.
---

# Key Guidelines

## Model Selection

When the host agent supports model selection for this task, prefer a low-cost, fast model from those available in that environment, provided it can perform the task reliably. Use the host's documented model identifiers and selection mechanism; do not assume a particular provider or invent model names or parameters. Respect explicit user and project model settings. If model selection is unavailable or no suitable alternative is known, keep the current model. Applying these guidelines to an ongoing development task does not require switching models or launching a sub-agent.

## Design Principles
- **DRY (Don't Repeat Yourself)**: Avoid code duplication
- **KISS (Keep It Simple, Stupid)**: Keep designs and code simple
- **YAGNI (You Ain't Gonna Need It)**: Don't implement features until actually needed. Extract methods/functions only when there's a concrete need for reuse, not in anticipation of it
- **SOLID Principles**: Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion
- **SoC (Separation of Concerns)**: Separate system by concerns (UI, business logic, data access, etc.)

## Development Methodology
- **TDD Approach**: Use TDD methodology (Kent Beck style) to break down tasks during execution
- **Micro-commits**: One change per commit, strictly follow test-driven change cycles (Lucas Rocha's micro-commit methodology)

## Quality & Standards
- **Code Quality**: Run linters and type checkers before committing
- **Security**: Always follow security guidelines and scan for vulnerabilities
- **Documentation**: Keep technical documentation in `docs/` directory
- **Version Control**: Follow conventional commit messages and branching strategy
