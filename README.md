# AI SDLC Scaffold

A phase-based project template designed for **AI-assisted software development** with [Claude Code](https://claude.ai/code).

## What Is This?

This scaffold provides a structured approach to building software projects with AI assistance. It organizes work into four phases — Objectives, Design, Code, Deploy — each with its own AI instruction file (`CLAUDE.<phase>.md`) that teaches Claude Code how to help effectively in that phase.

## Quick Start

1. **Copy this folder** as the starting point for a new project
2. **Customize `CLAUDE.md`**: Fill in the project overview, shell environment, and remove/adapt sections that don't apply
3. **Start with objectives**: Define what you're building in `1-objectives/`
4. **Design the system**: Document architecture and decisions in `2-design/`
5. **Build it**: Implement and track tasks in `3-code/`
6. **Ship it**: Deploy and operate from `4-deploy/`

## Structure

```
├── CLAUDE.md                     # Root AI instructions (start here)
│
├── 1-objectives/                # WHAT and WHY
│   ├── CLAUDE.objectives.md      # AI instructions for this phase
│   ├── goals.md                  # Project goals (template)
│   ├── requirements.md           # Requirements (template)
│   └── constraints.md            # Constraints (template)
│
├── 2-design/                    # HOW
│   ├── CLAUDE.design.md          # AI instructions for this phase
│   └── decisions/                # Architecture Decision Records
│       └── README.md             # ADR template and conventions
│
├── 3-code/                      # BUILD
│   ├── CLAUDE.code.md            # AI instructions for this phase
│   ├── tasks.md                  # Development task tracker
│   └── decisions/                # Development decisions (impl patterns)
│       └── README.md             # DEC template and conventions
│
├── 4-deploy/                    # SHIP
│   └── CLAUDE.deploy.md          # AI instructions for this phase
│
└── .claude/
    └── skills/
        └── next-task/
            └── SKILL.md          # Automated task execution skill
```

## Key Concepts

### Phase-Based Development
Each phase has a dedicated directory and a `CLAUDE.<phase>.md` instruction file. Phase instructions **extend** the root `CLAUDE.md` — they add phase-specific guidance without overriding the global rules.

### Two-Level Decision System
- **Architecture Decisions (ADRs)** in `2-design/decisions/`: High-level technology and design choices (e.g., "use PostgreSQL", "adopt microservices")
- **Development Decisions (DECs)** in `3-code/decisions/`: Implementation patterns that code must follow (e.g., "all delete operations cascade", "errors use structured format")

### API Contract Enforcement
If the project has separate frontend and backend codebases, the scaffold includes a contract verification procedure to keep type definitions in sync. Remove or customize this section in `CLAUDE.md` and `CLAUDE.code.md` if not applicable.

### Automated Task Execution
The `.claude/skills/next-task/` skill enables Claude Code to automatically pick and execute the next pending task from `3-code/tasks.md`. It handles complexity assessment, decision checking, and contract verification.

## Customization Checklist

When starting a new project from this scaffold:

- [ ] Update `CLAUDE.md` project overview section
- [ ] Configure shell environment in `CLAUDE.md` (nvm, pyenv, etc.)
- [ ] Customize or remove the API Contract section if not applicable
- [ ] Fill in `1-objectives/goals.md` with your project goals
- [ ] Fill in `1-objectives/requirements.md` with your requirements
- [ ] Fill in `1-objectives/constraints.md` with your constraints
- [ ] Customize `3-code/CLAUDE.code.md` directory structure and build commands
- [ ] Customize `3-code/tasks.md` with your initial task breakdown
- [ ] Review and adapt all `CLAUDE.*.md` files for your tech stack

## License
Licensed under the Apache License, Version 2.0. See `LICENSE`.

Contributions are accepted under the same license (inbound = outbound). See `CONTRIBUTING.md`.
