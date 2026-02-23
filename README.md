# AI SDLC Scaffold

A repository template for **AI-first software development** with [Claude Code](https://claude.ai/code).

## What Is This?

This scaffold provides a structured approach to building software where AI agents do the development work and a human supervises at a high level. It organizes work into four phases — **Objectives, Design, Code, Deploy** — each with an AI instruction file (`CLAUDE.<phase>.md`) that tells agents what to check, when to check it, and what to do.

The scaffold is built on four core principles:

1. **AI-first development model** — designed for AI agents doing the work while a human supervises, defines objectives, and steers direction.
2. **Everything-in-repo** — objectives, requirements, architecture, decisions, and task tracking all live alongside the source code, versioned and always accessible to agents without external tools.
3. **Context-window efficiency** — hierarchical instructions, two-file decision records, and phase-level indexes minimize how many tokens an agent must load.
4. **Decision capture, not suppression** — agents decide autonomously within established patterns; all decisions are recorded in the repository for human review and consistency.

For a deeper discussion of the rationale, design choices, and traceability model, see [`RATIONALE.md`](RATIONALE.md).

## Quick Start

1. **Copy this repository** as the starting point for a new project
2. **Customize `CLAUDE.md`**: fill in the project overview (look for `<!-- CUSTOMIZE -->` markers)
3. **Follow the phases**: Objectives → Design → Code → Deploy. Each phase has a `CLAUDE.<phase>.md` with instructions and indexes that guide the agent automatically.
4. **Use Claude skills** *(coming soon)*: dedicated skills will automate each phase — from requirements gathering to coding task execution — so you can navigate the lifecycle without manual prompting.

## Structure

```
├── CLAUDE.md                         # Root AI instructions (start here)
├── RATIONALE.md                      # Core principles and design rationale
│
├── 1-objectives/                     # WHAT and WHY
│   ├── CLAUDE.objectives.md          # Phase instructions and artifact indexes
│   ├── stakeholders.md               # Stakeholder definitions
│   ├── goals/                        # GOAL-NNN-name.md + _template.md
│   ├── user-stories/                 # US-NNN-name.md + _template.md
│   ├── requirements/                 # REQ-{PREFIX}NNN-name.md + _template.md
│   ├── assumptions/                  # ASM-NNN-name.md + _template.md
│   └── constraints/                  # CON-NNN-name.md + _template.md
│
├── 2-design/                         # HOW
│   ├── CLAUDE.design.md              # Phase instructions and decisions index
│   ├── architecture.md               # System architecture overview
│   ├── data-model.md                 # Data structures and schemas
│   ├── api-design.md                 # API specifications
│   └── decisions/                    # DEC-NNN-name.md + DEC-NNN-name.history.md
│       └── _template.md / _template.history.md
│
├── 3-code/                           # BUILD
│   ├── CLAUDE.code.md                # Phase instructions, decisions index, component guidelines
│   ├── tasks.md                      # Development task tracker
│   └── <codebase>/                   # One or more named codebases
│
└── 4-deploy/                         # SHIP
    ├── CLAUDE.deploy.md              # Phase instructions
    ├── infrastructure/               # Infrastructure as Code
    ├── scripts/                      # Deployment scripts
    └── runbooks/                     # Operational procedures + _template.md
```

## Key Concepts

- **Phase-based development**: each phase has a directory and a `CLAUDE.<phase>.md` file that extends the root `CLAUDE.md`. Phase gates define minimum preconditions before advancing.
- **Traceability**: every artifact references others by ID (`GOAL-001`, `REQ-F001`, `DEC-001`), creating a chain from business need to running code.
- **Two-file decisions**: active record (`DEC-NNN.md`) for enforcement, history file (`DEC-NNN.history.md`) for audit trail. Indexed per phase with trigger conditions.
- **Context-window efficiency**: hierarchical instructions, phase-level indexes, and the active/history split minimize how many tokens an agent needs to load.

## Customization Checklist

When starting a new project from this scaffold:

- [ ] Fill in the project overview in `CLAUDE.md` (search for `<!-- CUSTOMIZE -->`)
- [ ] Define stakeholders in `1-objectives/stakeholders.md`
- [ ] Customize `3-code/CLAUDE.code.md` with component guidelines and build commands
- [ ] Review the example decision (`DEC-001`) — adapt or replace it for your project
- [ ] Review all `CLAUDE.*.md` files for your tech stack

<!-- TODO: once automation skills are available, add a "Skills" section listing
     each skill (e.g. requirements gathering, task execution) with usage examples. -->

> **Note:** An automation layer of Claude skills is under active development. When ready, it will provide dedicated commands for requirements gathering, task planning, coding execution, and phase-gate navigation — reducing custom prompting to a minimum.

## License

Licensed under the Apache License, Version 2.0. See [`LICENSE`](LICENSE).

Contributions are accepted under the same license (inbound = outbound). See [`CONTRIBUTING.md`](CONTRIBUTING.md).
