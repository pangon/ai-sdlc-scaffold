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
2. **Run `/init`**: the initialization skill walks you through all customization points — project description, stakeholders, components, environments, and example decision review.
3. **Follow the phases**: Objectives → Design → Code → Deploy. Each phase has a `CLAUDE.<phase>.md` with instructions and indexes that guide the agent automatically.
4. **Use Claude skills**: type `/` followed by a skill name to automate each phase — from requirements gathering to coding task execution — without manual prompting. See the [Skills](#skills) section below.

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
├── 4-deploy/                         # SHIP
│   ├── CLAUDE.deploy.md              # Phase instructions
│   ├── infrastructure/               # Infrastructure as Code
│   ├── scripts/                      # Deployment scripts
│   └── runbooks/                     # Operational procedures + _template.md
│
└── .claude/skills/                   # Claude Code skills (automation layer)
    ├── init.md                       # /init — guided project initialization
    ├── elicit.md                     # /elicit — requirements elicitation
    ├── review-objectives.md          # /review-objectives — objectives dashboard
    ├── design.md                     # /design — design documents
    ├── decide.md                     # /decide — decision management
    ├── plan-tasks.md                 # /plan-tasks — task generation
    ├── implement.md                  # /implement — task execution
    ├── fix.md                        # /fix — bug-fix workflow
    ├── deploy.md                     # /deploy — deployment artifacts
    ├── status.md                     # /status — project dashboard
    ├── phase-gate.md                 # /phase-gate — gate checks
    └── trace.md                      # /trace — traceability walker
```

## Key Concepts

- **Phase-based development**: each phase has a directory and a `CLAUDE.<phase>.md` file that extends the root `CLAUDE.md`. Phase gates define minimum preconditions before advancing.
- **Traceability**: every artifact references others by ID (`GOAL-001`, `REQ-F001`, `DEC-001`), creating a chain from business need to running code.
- **Two-file decisions**: active record (`DEC-NNN.md`) for enforcement, history file (`DEC-NNN.history.md`) for audit trail. Indexed per phase with trigger conditions.
- **Context-window efficiency**: hierarchical instructions, phase-level indexes, and the active/history split minimize how many tokens an agent needs to load.

## Customization Checklist

When starting a new project from this scaffold:

Run `/init` to complete these steps interactively, or do them manually:

- [ ] Fill in the project overview in `CLAUDE.md` (search for `<!-- CUSTOMIZE -->`)
- [ ] Define stakeholders in `1-objectives/stakeholders.md`
- [ ] Customize `3-code/CLAUDE.code.md` with component guidelines and build commands
- [ ] Review the example decision (`DEC-001`) — adapt or replace it for your project
- [ ] Review all `CLAUDE.*.md` files for your tech stack

## Skills

Claude skills automate each phase of the lifecycle. Type `/skill-name` in Claude Code to invoke them. Each skill reads the root `CLAUDE.md` and the relevant phase instructions before acting.

### Setup

| Skill | Purpose |
|-------|---------|
| `/init` | Guided project initialization — walks through all customization points: project description, stakeholders, components, environments, example decision review, and tech-stack adjustments. |

### Objectives Phase

| Skill | Purpose |
|-------|---------|
| `/elicit` | Interactive requirements elicitation — guides you through stakeholders, goals, assumptions, constraints, user stories, and requirements in the prescribed order. Creates artifacts from templates, updates indexes. |
| `/review-objectives` | Read-only dashboard of all objectives artifacts. Shows counts by status, traceability gaps, conflicts, and Objectives→Design gate readiness. |

### Design Phase

| Skill | Purpose |
|-------|---------|
| `/design` | Draft or update architecture, data model, or API design documents based on approved requirements. References requirements by ID for traceability. |
| `/decide` | Record, review, deprecate, or supersede design decisions. Creates both `DEC-NNN.md` and `DEC-NNN.history.md`, updates all phase indexes. |

### Code Phase

| Skill | Purpose |
|-------|---------|
| `/plan-tasks` | Generate development tasks from approved requirements and design. Populates `tasks.md` with IDs, priorities, requirement links, and an execution plan. |
| `/implement` | Execute the next task (or a specified `TASK-NNN`) following the full procedure: read requirements, check decisions, implement with tests, handle design gaps, update status. |
| `/fix` | Structured bug-fix workflow: write failing test, fix, check for same pattern elsewhere, ask before fixing other occurrences. |

### Deploy Phase

| Skill | Purpose |
|-------|---------|
| `/deploy` | Generate or update IaC, deployment scripts, and runbooks. Flags cost drivers, enforces idempotency, links to requirements. |

### Cross-Phase

| Skill | Purpose |
|-------|---------|
| `/status` | Project-wide dashboard: artifact counts per phase, task progress, decision summary, and phase-gate readiness for every transition. |
| `/phase-gate` | Check preconditions for a specific phase transition (e.g., `/phase-gate design`). Reports MET/NOT MET per precondition with evidence. Gates are advisory — warns but proceeds on user confirmation. |
| `/trace` | Given any artifact ID, walks the traceability chain upstream (why does this exist?) and downstream (what depends on this?). Reports gaps and broken links. |

### Typical Workflow

```
/init                ← set up project: description, stakeholders, components, environments
/elicit              ← define stakeholders, goals, requirements (Draft)
/review-objectives   ← check completeness
                     ← human approves artifacts (Draft → Approved)
/phase-gate design   ← verify gate
/design              ← draft architecture, data model, API
/decide              ← capture decisions as they emerge
/phase-gate code     ← verify gate
/plan-tasks          ← generate task backlog
/implement           ← execute tasks one by one
/fix                 ← handle bugs
/phase-gate deploy   ← verify gate
/deploy              ← generate deployment artifacts
/status              ← anytime: full project overview
/trace REQ-F001      ← anytime: follow dependency chains
```

## License

Licensed under the Apache License, Version 2.0. See [`LICENSE`](LICENSE).

Contributions are accepted under the same license (inbound = outbound). See [`CONTRIBUTING.md`](CONTRIBUTING.md).
