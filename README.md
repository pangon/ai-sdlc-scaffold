# AI SDLC Scaffold

A phase-based project template designed for **AI-assisted software development** with [Claude Code](https://claude.ai/code).

## What Is This?

This scaffold provides a structured approach to building software projects with AI assistance. It organizes work into four phases — **Objectives, Design, Code, Deploy** — each with its own AI instruction file (`CLAUDE.<phase>.md`) that teaches Claude Code how to help effectively in that phase.

The central paradigm is **everything-in-repo**: objectives, requirements, architecture, decisions, and task tracking all live alongside the source code — versioned, searchable, and always in sync with the implementation. This makes the full history of *why* the software is built the way it is available to both humans and AI assistants at all times.

## Quick Start

1. **Copy this repository** as the starting point for a new project
2. **Customize `CLAUDE.md`**: fill in the project overview and remove/adapt sections that don't apply
3. **Start with objectives**: define stakeholders, goals, and requirements in `1-objectives/`
4. **Design the system**: document architecture and record decisions in `2-design/`
5. **Build it**: implement and track tasks in `3-code/`
6. **Ship it**: deploy and operate from `4-deploy/`

## Structure

```
├── CLAUDE.md                         # Root AI instructions (start here)
│
├── 1-objectives/                     # WHAT and WHY
│   ├── CLAUDE.objectives.md          # Phase instructions, stakeholder list, artifact indexes
│   ├── goals/                        # Goal files (GOAL-NNN-name.md) + _template.md
│   ├── user-stories/                 # User stories (US-NNN-name.md) + _template.md
│   ├── requirements/                 # Requirements (REQ-{type}NNN-name.md) + _template.md
│   ├── assumptions/                  # Assumptions (ASM-NNN-name.md) + _template.md
│   └── constraints/                  # Constraints (CON-NNN-name.md) + _template.md
│
├── 2-design/                         # HOW
│   ├── CLAUDE.design.md              # Phase instructions and decisions index
│   ├── architecture.md               # System architecture overview
│   ├── data-model.md                 # Data structures and schemas
│   ├── api-design.md                 # API specifications
│   └── decisions/                    # Decision Records (DEC-NNN-name.md + .history.md)
│       └── _template.md / _template.history.md
│
├── 3-code/                           # BUILD
│   ├── CLAUDE.code.md                # Phase instructions, decisions index, component guidelines
│   ├── tasks.md                      # Development task tracker
│   └── <codebase>/                   # One or more named codebases (e.g. frontend, backend)
│
└── 4-deploy/                         # SHIP
    ├── CLAUDE.deploy.md              # Phase instructions
    ├── infrastructure/               # Infrastructure as Code
    ├── scripts/                      # Deployment scripts
    └── runbooks/                     # Operational procedures
```

## Key Concepts

### Phase-Based Development

Each phase has a dedicated directory and a `CLAUDE.<phase>.md` instruction file. Phase instructions **extend** the root `CLAUDE.md` — they add phase-specific guidance without overriding the global rules.

### Everything-in-Repo Traceability

Every artifact — from stakeholder definitions through goals, user stories, requirements, design documents, decisions, and tasks — is a versioned file in the repository. Artifacts reference each other by ID (e.g., `GOAL-001`, `REQ-F001`, `DEC-002`), creating a traceable chain from business need to running code.

### Unified Decision Records

All decisions live in `2-design/decisions/`, regardless of which phase surfaced them. Each decision consists of two files:

- **`DEC-NNN-name.md`** — the active record: context, decision, and enforcement rules
- **`DEC-NNN-name.history.md`** — the trail: alternatives considered, reasoning, and changelog

Decisions are indexed in each phase's `CLAUDE.<phase>.md` with phase-specific trigger conditions, so AI agents know when to check and apply them.

### Structured Objectives

The `1-objectives/` phase uses individual files per artifact (one file per goal, per user story, per requirement, etc.) with index tables in `CLAUDE.objectives.md`. This keeps each artifact self-contained while providing a scannable overview.

## Customization Checklist

When starting a new project from this scaffold:

- [ ] Update the project overview section in `CLAUDE.md`
- [ ] Replace placeholder stakeholders in `1-objectives/CLAUDE.objectives.md`
- [ ] Replace placeholder goals, user stories, requirements, assumptions, and constraints
- [ ] Customize `3-code/CLAUDE.code.md` with your component guidelines and build commands
- [ ] Set up initial tasks in `3-code/tasks.md`
- [ ] Review and adapt all `CLAUDE.*.md` files for your tech stack
- [ ] Remove or customize decision records that don't apply (e.g., DEC-001 API contract, DEC-002 Node.js version management, DEC-003 TypeScript verification)

## License

Licensed under the Apache License, Version 2.0. See [`LICENSE`](LICENSE).

Contributions are accepted under the same license (inbound = outbound). See [`CONTRIBUTING.md`](CONTRIBUTING.md).
