# AI SDLC Scaffold

A repository template for **AI-first software development** with [Claude Code](https://claude.ai/code) that organizes the entire software development lifecycle (SDLC) into four phases — Specification, Design, Code, Deploy — each with folder structure, artifacts, and agent instructions that encode what to check, when to check it, and what to do, so that all project knowledge lives inside the repository and a human can supervise at a high level.

It is not a tool, a library, or an application. It is a **starting scaffold** — you clone or copy it, then fill it in as your real project takes shape.

The scaffold is built on four core principles:

1. **AI-first development model** — designed for AI agents doing the work while a human supervises, defines specification, and steers direction.
2. **Everything-in-repo** — specification, requirements, architecture, decisions, and task tracking all live alongside the source code, versioned and always accessible to agents without external tools.
3. **Context-window efficiency** — hierarchical instructions, two-file decision records, and phase-level indexes minimize how many tokens an agent must load.
4. **Decision capture, not suppression** — agents decide autonomously within established patterns; all decisions are recorded in the repository for human review and consistency.

> For a deeper discussion of the rationale, core principles, and design choices see [`RATIONALE.md`](RATIONALE.md).

## Quick Start

### Option A — Use `degit` (recommended)

```bash
npx degit pangon/ai-sdlc-scaffold my-project
cd my-project
rm -f CONTRIBUTING.md CONTRIBUTORS.md LICENSE NOTICE RATIONALE.md README.md
git init && git add -A && git commit -m "Initial scaffold"
```

[`degit`](https://github.com/Rich-Harris/degit) copies the repository contents without carrying over git history, giving you a clean starting point. It requires Node.js but does not install any dependency in your project. The `rm` step removes repo-specific files that are not part of the scaffold.

### Option B — Manual copy

1. Download or clone this repository.
2. Copy all files and directories (including hidden ones like `.claude/`) into your new project folder.
3. Remove the `.git/` directory to start with a fresh history.
4. Remove repo-specific files: `rm -f CONTRIBUTING.md CONTRIBUTORS.md LICENSE NOTICE RATIONALE.md README.md`.
5. Initialize a new repository: `git init && git add -A && git commit -m "Initial scaffold"`.

### Then

1. **Run `/SDLC-init`** in Claude Code: the initialization skill sets up the project description in `CLAUDE.md` so agents have the right context from the start.
2. **Work through the phases** (Specification → Design → Code → Deploy) using the built-in [skills](#skills) — type `/` followed by a skill name to automate each step, from requirements gathering to deployment. Alternatively, you can use custom prompts: the `CLAUDE.<phase>.md` hierarchy provides all the context the agent needs to operate correctly in each phase.

> **Manual edits are strongly discouraged.** All changes to artifacts and code should go through an AI agent whose behavior is governed by the instruction files, existing artifacts, and ideally a skill. This ensures that procedures are followed, decisions are recorded, and the system stays consistent.

## Structure

```
├── CLAUDE.md                         # Root AI instructions (start here)
├── RATIONALE.md                      # Core principles and design rationale
│
├── 1-spec/                           # WHAT and WHY
│   ├── CLAUDE.spec.md                # Phase instructions, decisions index, artifact indexes
│   ├── stakeholders.md               # Stakeholder definitions
│   ├── goals/                        # GOAL-kebab-name.md + _template.md
│   ├── user-stories/                 # US-kebab-name.md + _template.md
│   ├── requirements/                 # REQ-CLASS-kebab-name.md + _template.md
│   ├── assumptions/                  # ASM-kebab-name.md + _template.md
│   └── constraints/                  # CON-kebab-name.md + _template.md
│
├── 2-design/                         # HOW
│   ├── CLAUDE.design.md              # Phase instructions and decisions index
│   ├── architecture.md               # System architecture overview
│   ├── data-model.md                 # Data structures and schemas
│   └── api-design.md                 # API specifications
│
├── 3-code/                           # BUILD
│   ├── CLAUDE.code.md                # Phase instructions, decisions index, component guidelines
│   ├── tasks.md                      # Development task tracker
│   └── <component>/                  # Per-component directories (created by /SDLC-decompose)
│       └── CLAUDE.<component>.md     # Component description, decisions index, addressed requirements
│
├── 4-deploy/                         # SHIP
│   ├── CLAUDE.deploy.md              # Phase instructions, decisions index
│   └── runbooks/                     # Operational procedures + _template.md
│
├── decisions/                        # DEC-kebab-name.md + DEC-kebab-name.history.md
│
└── .claude/skills/                   # Claude Code skills (automation layer)
    ├── SDLC-init/SKILL.md            # guided project initialization
    ├── SDLC-elicit/SKILL.md          # requirements elicitation
    ├── SDLC-design/SKILL.md          # design documents
    ├── SDLC-decompose/SKILL.md       # component identification
    ├── SDLC-implementation-plan/SKILL.md  # task generation
    ├── SDLC-execute-next-task/SKILL.md    # task execution
    ├── SDLC-fix/SKILL.md             # bug fixes and ad-hoc changes
    └── SDLC-status/SKILL.md          # /SDLC-status — project dashboard
```

## Key Concepts

- **Phase-based development**: each phase has a directory and a `CLAUDE.<phase>.md` file that extends the root `CLAUDE.md`. Phase gates define minimum preconditions before advancing.
- **Traceability**: every artifact references others by descriptive ID (`GOAL-reduce-latency`, `REQ-F-search-by-name`, `DEC-use-postgres`), creating a chain from business need to running code. Index tables link directly to artifact files for easy navigation.
- **Two-file decisions**: active record (`DEC-kebab-name.md`) for enforcement, history file (`DEC-kebab-name.history.md`) for audit trail. Indexed per phase with trigger conditions.
- **Context-window efficiency**: hierarchical instructions, phase-level indexes, and the active/history split minimize how many tokens an agent needs to load.

## How the Scaffold Gets Populated

The scaffold starts empty. Project-specific artifacts are created progressively as you work through each phase — not all at once.

- **At project start** (`/SDLC-init`): set the project description in `CLAUDE.md` so agents have the right context from the start.
- **During the Specification phase** (`/SDLC-elicit`): define stakeholders, goals, constraints, assumptions, user stories, and requirements.
- **During the Design phase** (`/SDLC-design`): draft architecture, data model, API design. Decisions are recorded as they emerge during design work.
- **At the start of the Code phase** (`/SDLC-decompose`, `/SDLC-implementation-plan`): identify components (creating per-component directories under `3-code/`), then generate the task backlog.
- **During the Code phase** (`/SDLC-execute-next-task`, `/SDLC-fix`): implement tasks one at a time (each call picks the next pending task, writes code and tests, and updates the task status), or apply bug fixes and ad-hoc changes as they arise.

Claude skills automate each phase of the lifecycle. Type `/skill-name` in Claude Code to invoke them. Each skill reads the root `CLAUDE.md` and the relevant phase instructions before acting.

### Setup

| Skill | Purpose |
|-------|---------|
| `/SDLC-init` | Guided project initialization — sets the project description in `CLAUDE.md` so agents have the right context from the start. |

### Specification Phase

| Skill | Purpose |
|-------|---------|
| `/SDLC-elicit` | Interactive requirements elicitation — guides you through stakeholders, goals, assumptions, constraints, user stories, and requirements in the prescribed order. Creates artifacts from templates, updates indexes. Also performs gap analysis on existing artifacts. |

### Design Phase

| Skill | Purpose |
|-------|---------|
| `/SDLC-design` | Draft or update architecture, data model, or API design documents based on approved requirements. Covers design completeness assessment. References requirements by ID for traceability. |
| `/SDLC-decompose` | Identify distinct software components from design artifacts and create per-component directories under `3-code/`. Use when transitioning from Design to Code phase. |

### Code Phase

| Skill | Purpose |
|-------|---------|
| `/SDLC-implementation-plan` | Create a phased implementation plan from design artifacts. Populates `tasks.md` with short tasks grouped into incremental development phases, each ending with a deployable/testable system. |
| `/SDLC-execute-next-task` | Execute the next pending task from the implementation plan. Finds the first actionable task, implements with tests, handles design gaps, and updates task status. |
| `/SDLC-fix` | Apply a user-reported fix, bug correction, or ad-hoc change. Gathers context interactively, identifies affected components, checks design artifacts and decisions, implements with tests, and handles design gaps. |

### Cross-Phase

| Skill | Purpose |
|-------|---------|
| `/SDLC-status` | Project-wide dashboard: artifact counts per phase, task progress, phase-gate readiness, traceability health, and assessment freshness. |

### Typical Workflow

```
/SDLC-init                  ← set up project
/SDLC-elicit                ← define stakeholders, goals, requirements
                            ← human approves artifacts (Draft → Approved)
/SDLC-design                ← draft architecture, data model, API design
/SDLC-decompose             ← identify components, create directories
/SDLC-implementation-plan   ← generate task backlog
/SDLC-execute-next-task     ← execute tasks one by one
/SDLC-fix                   ← apply bug fixes or ad-hoc changes
/SDLC-status                ← anytime: full project overview
```

## License

Licensed under the Apache License, Version 2.0. See [`LICENSE`](LICENSE).

Contributions are accepted under the same license (inbound = outbound). See [`CONTRIBUTING.md`](CONTRIBUTING.md).
