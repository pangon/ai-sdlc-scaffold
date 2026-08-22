---
name: SDLC-decompose
description: Identify distinct software components from design artifacts and create per-component directories. Use when transitioning from Design to Code phase.
---

## Instructions

You are working at the Design → Code transition, analyzing design artifacts to identify distinct software components, creating per-component directories in the Code phase, and documenting them in the Code phase instructions.

### Phase Validation

Before doing anything else, read the `**Phase**:` field of the `### Current State` subsection in `CLAUDE.md`. Then follow the matching case below:

1. **`Not initialized`** — **Stop**, recommend `/SDLC-init`, and do not proceed.

2. **`Specification`** — **Stop**, recommend `/SDLC-elicit` to continue refining specification or `/SDLC-design` to start the design phase, and do not proceed.

3. **`Design`** — decomposition would cross the Design → Code phase gate. Evaluate the Design → Code preconditions from the Phase Gates table in `CLAUDE.md` (using its "How to verify" column), except "Components identified" — that precondition is what this skill establishes. Then respond based on the results:

   - **If the design documents are not all drafted or `N/A`** — design is incomplete. **Stop**, list which documents are missing or incomplete, recommend `/SDLC-design` to continue drafting, and do not proceed.
   - **If the documents are drafted but the completeness-assessment precondition is not met** (no assessment, stale, or open Critical findings) — **strongly recommend** running a Completeness Assessment via `/SDLC-design` before decomposition. Proceed only if the user explicitly accepts the gaps.
   - **If all evaluated preconditions are met** — inform the user of any remaining Important/Minor findings (non-blocking) and **ask the user to confirm the advancement to the Code phase** (phase gate advancement is an "always ask" action). Proceed with Setup only after confirmation.

   In every proceed case, the `**Phase**:` field is set to `Code` when the component directories are created (see Current State Tracking) — not at validation time.

4. **`Code`** — components may already exist. **Warn** that reorganizing components can heavily impact downstream tasks and existing code (task assignments, directory structure, imports, build configuration, and deployment pipelines may all need updating). If the user confirms, proceed but flag downstream dependencies that could be affected.

### Setup

1. Read `2-design/CLAUDE.design.md` (phase instructions and decisions index).
2. Read `2-design/architecture.md` — the primary source for component identification.
3. Read `2-design/api-design.md` — look for communication interfaces between components.
4. Read `2-design/data-model.md` — understand which components own which data.
5. Read `3-code/CLAUDE.code.md` — understand the current state of the Components section.
6. List existing directories under `3-code/` to avoid overwriting existing structure.
7. Read all the decisions in `decisions/`.

### Component Identification

The goal of this step is to identify parts of the system that benefit from being organized as **separate codebases** — with their own directory tree, dependencies, build toolchain, or test suite. Separating these parts makes it easier to develop, build, and test each one independently, and keeps the repository structure aligned with the system's architectural boundaries.

A **component** is a cohesive unit of software with:

- A defined responsibility boundary
- A communication interface with at least one other component (REST API, SSE, Python class API, file system protocol, message queue, etc.)
- An independently buildable or testable codebase (different language/framework, different build tool, or explicit module boundary)

**Identification heuristics** (apply in order of strength):

1. **Explicit interface boundaries** — If the architecture defines a communication protocol between two parts of the system (HTTP, SSE, Python class API, message queue), each side of the interface is a separate component.
2. **Different technology stacks** — Parts using different languages or frameworks (e.g., Python backend vs. JavaScript frontend) are separate components.
3. **"Standalone" or "modular" declarations** — The architecture may explicitly call out a module as independently usable or having a clean interface boundary.
4. **Distinct build/test toolchains** — Parts with different package managers, test frameworks, or build tools are separate components.

For each identified component, collect:

- **Name**: a short, descriptive identifier (kebab-case, e.g., `backend`, `frontend`, `tts-engine`)
- **Responsibility**: what it does (from the architecture)
- **Technology**: primary language/framework
- **Interfaces**: how it communicates with other components (protocol, direction, summary)
- **Requirements addressed**: which `REQ-*` IDs it covers (from the requirement traceability section of the architecture or from cross-referencing the design)
- **Relevant decisions**: which `DEC-*` apply to this component

### Workflow

#### 1. Present Identified Components

Present the identified components to the user in a table:

| Component | Responsibility | Technology | Interfaces |
|-----------|---------------|------------|------------|
| ... | ... | ... | ... |

For each component, briefly explain which heuristic(s) matched and what evidence from the design artifacts supports the identification.

If any component boundary is ambiguous (e.g., a module could be part of a larger component or independent), present the alternatives and ask the user to decide.

**Wait for user approval** before proceeding. The user may merge, split, rename, or redefine components.

#### 2. Create Component Directories

For each approved component, create a directory and a `CLAUDE.md` inside it:

```
3-code/<component-name>/CLAUDE.md
```

The file is named exactly `CLAUDE.md` so that Claude Code auto-loads it whenever it accesses files inside the component directory — the component's instructions and decisions index are then in effect even when work happens outside a skill. Do not use component-specific names (e.g., `CLAUDE.backend.md`): auto-loading and component discovery depend on the literal name.

The component's `CLAUDE.md` file should contain:

```markdown
# <Component Display Name>

**Responsibility**: <one-line summary from architecture>

**Technology**: <primary language/framework and key libraries>

## Interfaces

- <protocol> with <other-component>: <brief description>

## Requirements Addressed

| File | Type | Priority | Summary |
|------|------|----------|---------|
| [REQ-*](../../1-spec/requirements/REQ-*.md) | ... | ... | ... |

## Relevant Decisions

| File | Title | Trigger |
|------|-------|---------|
| [DEC-*](../../decisions/DEC-*.md) | ... | ... |
```

Do **not** move or restructure existing files in `2-design/`. The shared design documents (`architecture.md`, `data-model.md`, `api-design.md`) remain in their phase directory. Component directories in `3-code/` supplement them with component-specific focus.

#### 3. Update 3-code/CLAUDE.code.md

Replace the placeholder content in the `## Components` section with an entry for each component. Use this format:

```markdown
### <Component Display Name>

- **Directory**: [`<component-name>/`](<component-name>/)
- **Technology**: <primary language/framework>
- **Responsibility**: <one-line summary from architecture>
```

Keep the section concise — it is a navigation aid, not a design document.

#### 4. Next Step Guidance

After the component structure is created and `CLAUDE.code.md` is updated, inform the user that the next step is **implementation planning** via `/SDLC-implementation-plan`. This skill reads the design artifacts and component definitions to produce a phased sequence of short development tasks in `3-code/tasks.md`, each phase ending with a deployable or testable system.

### Interaction Style

- Present all identified components at once (they are closely related) — do not ask one at a time.
- Wait for user approval before creating any directories or files.
- After creating the structure, summarize what was done and ask the user if any adjustments are needed.

### Current State Tracking

After creating the approved component directories, update the `### Current State` subsection in `CLAUDE.md` following the Current State Protocol defined there: set `**Phase**: Code` (the advancement was confirmed by the user during Phase Validation), update `**Summary**:` to note the transition, convert the `**Completeness assessment**:` line to its passed form, recording how the gate was passed (protocol rule 3), and add the `**Components**:` line with the comma-separated component names (e.g., `**Components**: backend, frontend, tts-engine`).

### Rules

- **Read-before-write**: always read existing files before proposing changes.
- **Do not move existing files**: design documents remain at the `2-design/` top level.
- **Only create skeleton directories**: the skill creates per-component directories, each with a `CLAUDE.md`, in `3-code/`. Actual source code, build configuration, and test scaffolding are added during implementation.
- **Preserve existing content**: when updating `CLAUDE.code.md`, do not remove or modify content outside the sections this skill manages.
- **User approval required**: present identified components and wait for explicit approval before writing any files.
