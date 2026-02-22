# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with this repository.

## Language Policy

**All AI outputs must be in English**, regardless of the language used in user prompts. This applies to:
- Code and inline comments
- Documentation (markdown files, README, etc.)
- Configuration files and their comments
- Commit messages
- Response text and explanations

The user may write prompts in any language, but Claude must always respond and produce artifacts in English.

---

## Project Overview

<!-- TODO: Describe your project here -->
This project follows a phase-based development lifecycle with dedicated directories for each phase, designed for AI-assisted software development with Claude Code.

---

## Repository Structure

The repository is organized into development phases. Claude should respect this organization and guide users to the appropriate section.

```
project/
├── CLAUDE.md                 # Main AI instructions (this file)
├── README.md                 # Project overview and quick start
│
├── 01-objectives/            # High-level goals and requirements
│   ├── CLAUDE.objectives.md  # Phase-specific AI instructions
│   ├── goals.md              # Project goals and success criteria
│   ├── requirements.md       # Functional and non-functional requirements
│   └── constraints.md        # Technical and business constraints
│
├── 02-design/                # Architecture and design decisions
│   ├── CLAUDE.design.md      # Phase-specific AI instructions
│   ├── architecture.md       # System architecture overview
│   ├── data-model.md         # Data structures and schemas
│   ├── api-design.md         # API specifications
│   └── decisions/            # Architecture Decision Records (ADRs)
│
├── 03-code/                  # Implementation
│   ├── CLAUDE.code.md        # Phase-specific AI instructions
│   ├── tasks.md              # Development task tracker
│   ├── decisions/            # Development decisions (implementation patterns)
│   ├── src/                  # Source code
│   └── tests/                # Test suites
│
└── 04-deploy/                # Deployment and operations
    ├── CLAUDE.deploy.md      # Phase-specific AI instructions
    ├── infrastructure/       # IaC configurations
    ├── scripts/              # Deployment scripts
    └── runbooks/             # Operational procedures
```

---

## Phase-Specific Instructions

Each phase directory contains a `CLAUDE.<phase>.md` file with detailed instructions for that phase. Claude should:

1. **Check for phase-specific instructions** when working in a phase directory
2. **Follow the hierarchy**: phase-specific instructions extend (not override) this file
3. **Create phase files** when they don't exist and the user starts working on that phase

### Phase Workflow

| Phase | Directory | Focus |
|-------|-----------|-------|
| **Objectives** | `01-objectives/` | Define what to build and why |
| **Design** | `02-design/` | Define how to build it |
| **Code** | `03-code/` | Build it |
| **Deploy** | `04-deploy/` | Ship and operate it |

Claude should help users navigate between phases and ensure work is placed in the correct location.

---

## AI Collaboration Guidelines

### When Starting Work

1. Identify which phase the task belongs to
2. Check for phase-specific `CLAUDE.<phase>.md` instructions
3. Create necessary directories and files if they don't exist
4. Work within the appropriate phase structure

### During Development

- Keep documentation close to the code it describes
- Update relevant phase documents when making changes
- Cross-reference between phases when appropriate (e.g., link design docs from code)
- **Ask before deciding**: When you spot a related issue, potential improvement, or ambiguous situation during a task, **ask the user explicitly** instead of silently deciding to act or not act. Never assume the user's intent — always surface the decision. Example: "I noticed the same problem exists in file X — should I fix it too?"

### After Making Changes

Claude should evaluate whether to:

1. **Update this file** (`CLAUDE.md`) if:
   - New project-wide patterns emerge
   - Build commands or architecture change significantly
   - New tools or workflows are introduced

2. **Update phase-specific files** (`CLAUDE.<phase>.md`) if:
   - Phase-specific patterns or conventions are established
   - New tools specific to that phase are added
   - Common tasks in that phase should be documented

3. **Create new instruction files** if:
   - A subtask becomes complex enough to warrant dedicated guidance
   - A new phase or sub-phase emerges
   - Team-specific workflows need documentation

Claude should proactively suggest these updates when relevant.

---

## Shell Environment

<!-- TODO: Customize for your project's runtime environment -->
<!-- Example for nvm-based Node.js projects: -->
<!--
### Node.js Version Management (nvm)

This project uses **nvm** with `.nvmrc` files to pin Node.js versions per sub-project. Because nvm does not auto-load in non-interactive shells, **every Bash command that involves Node.js tooling** (npm, npx, node, tsc, etc.) must be prefixed with:

```bash
source ~/.nvm/nvm.sh && nvm use &&
```
-->

---

## API Contract (Frontend-Backend)

<!-- TODO: Remove this section if your project has no frontend-backend boundary, or customize it -->

If the project has separate frontend and backend codebases, they maintain **separate type definitions** that must stay in sync. Any code change that touches one side of the API boundary **must** verify and update the other side.

### Contract Principles

- **Response types must be explicit**: Every endpoint must have a named response type, derived from internal entity types. Never return raw internal entities over the wire.
- **Mapper functions**: Convert internal entities to response types, stripping internal fields and adding computed fields.
- **Frontend types mirror backend response types**: Field names, types, optionality, and enum values must match exactly.
- **Response envelope**: Standardize on a consistent response wrapper (e.g., `{ success: true, data: {...} }`).
- **List endpoints use named fields**: Return `{ users: [...] }`, `{ products: [...] }`, not generic `{ items: [...] }`.

### Mandatory Rule

**When producing code that creates or modifies an API endpoint, request/response type, or API client call, Claude must verify that the frontend-backend contract is consistent.** See `CLAUDE.code.md` for the detailed verification procedure.

---

## Development Decisions

Implementation-level decisions that ensure consistency across the codebase live in `03-code/decisions/`. These are separate from architecture decisions (ADRs) in `02-design/decisions/`:

- **Architecture decisions** (`02-design/decisions/ADR-*`): High-level technology and design choices
- **Development decisions** (`03-code/decisions/DEC-*`): Implementation patterns and conventions that all code must follow

### Mandatory Rule

**When executing any coding task, Claude must:**
1. **Check applicable decisions**: Scan the decisions index in `CLAUDE.code.md` and read any decisions relevant to the task. Apply them during implementation.
2. **Propose new decisions**: After completing a task, evaluate whether a new implementation pattern emerged that should be documented as a development decision to maintain consistency.

---

## Build and Development Commands

*See `CLAUDE.code.md` for detailed build commands per sub-project.*

---

## Architecture

*To be documented in `02-design/architecture.md`. This section will contain a summary with links to detailed documentation.*

---

## Navigation Help

When asked about the project, Claude should:

- **For "what are we building?"** -> Direct to `01-objectives/`
- **For "how does it work?"** -> Direct to `02-design/`
- **For "where is the code for X?"** -> Navigate `03-code/`
- **For "how do I deploy?"** -> Direct to `04-deploy/`

---

## Continuous Improvement

This file and all `CLAUDE.*.md` files are living documents. After significant work sessions, Claude should:

1. Review if current instructions are still accurate
2. Identify patterns that could be documented
3. Suggest improvements or new instruction files
4. Keep instructions concise and actionable
