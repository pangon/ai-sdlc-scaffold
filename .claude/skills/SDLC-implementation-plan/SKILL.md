---
name: SDLC-implementation-plan
description: Create a phased implementation plan from design artifacts. Populates tasks.md with short tasks grouped into incremental development phases, each ending with a deployable/testable system. Use after SDLC-decompose, before coding begins.
---

## Instructions

You are working in the Code phase. You are creating an **implementation plan** — a phased sequence of short development tasks that, when executed in order, build the project incrementally from nothing to full feature set. Each phase ends with a system that can be deployed or at least tested end-to-end, even if not feature-complete.

The plan populates `3-code/tasks.md` with tasks and an Execution Plan that groups them into ordered phases.

### Phase Validation

Before doing anything else, read the `### Current State` subsection under `## Project Overview` in `CLAUDE.md` and determine which phase the project is in. Then follow the matching case below:

1. **Project not initialized** — the Current State lacks a real project description (e.g., mentions "not yet been initialized" or "base scaffold"). **Stop**, recommend `/SDLC-init`, and do not proceed.

2. **Project is in the Specification phase** — the Current State mentions "Specification phase", lists specification artifacts being drafted, or no phase beyond Specification has been started. **Stop**, recommend `/SDLC-elicit` to continue refining specifications or `/SDLC-design` to start the design phase, and do not proceed.

3. **Project is in the Design phase without components identified** — **Stop**, recommend `/SDLC-decompose` first, and do not proceed.

4. **Project is in the Code phase with components identified but no tasks yet** — this is the expected entry point. Evaluate prerequisites:

   - **(a)** All design documents have content — `architecture.md`, `data-model.md`, and `api-design.md` are drafted
   - **(b)** Components are identified — per-component directories exist in `3-code/`
   - **(c)** `3-code/tasks.md` has no tasks yet (only the template)

   Then:
   - **If (a) or (b) not met** — **Stop**, explain what is missing.
   - **If (c) is not met** (tasks already exist) — **Warn** that running this skill will replace existing task entries and the Execution Plan. Proceed only on explicit user confirmation.
   - **If all met** — proceed with Setup.

5. **Project has tasks already in progress or done** — **Warn** strongly that re-planning may invalidate in-progress work. Proceed only on explicit user confirmation.

### Setup

Read the following files — all are required context for planning:

1. **Goals** — read every `GOAL-*.md` file in `1-spec/goals/`. Extract each goal's title, priority, status, and **Success Criteria** checkboxes. Only consider goals with `Status: Approved`.
2. **Requirements** — read every `REQ-*.md` file in `1-spec/requirements/`. Note their type (Functional, Performance, Security, etc.), priority, and linked goals/stories.
3. **Constraints** — read every `CON-*.md` file in `1-spec/constraints/`.
4. **Design documents** — read `2-design/architecture.md`, `2-design/data-model.md`, and `2-design/api-design.md`.
5. **Decisions** — read every `DEC-*.md` file in `decisions/` (active records only, not `.history.md`).
6. **Code phase instructions** — read `3-code/CLAUDE.code.md`.
7. **Components** — read every `CLAUDE.component.md` in `3-code/*/`.
8. **Deploy phase instructions** — read `4-deploy/CLAUDE.deploy.md`.

### Analysis

Before constructing the plan, perform these analyses silently (do not output them — they inform the plan):

#### 1. Requirement-to-Component Map

For each requirement, identify which component(s) must implement it (from the component `CLAUDE.component.md` files). Requirements that span multiple components need tasks in each.

#### 2. Dependency Graph

Identify ordering constraints between requirements (examples for each category):
- **Infrastructure dependencies**: e.g., database schema before data access, project scaffolding before features, build tooling before components
- **Data flow dependencies**: e.g., backend API before frontend consumption, authentication service before protected endpoints
- **Feature dependencies**: e.g., core CRUD before advanced features, basic listing before search/filter

#### 3. Priority Ordering

Group requirements by goal priority:
- **Must-have goals first** — their requirements form the earliest phases
- **Should-have goals later** — their requirements come after Must-have coverage
- **Could-have goals last**

#### 4. Cross-Cutting Concerns

Identify work that does not map to a single requirement but is needed for the system to function:
- Project scaffolding (package.json, pyproject.toml, directory structure, virtual environments)
- Build and dev tooling (dev server, hot reload, linters)
- Database initialization (schema creation, migrations)
- Integration wiring (backend serves frontend, CORS, static files)
- Infrastructure as code (Terraform, AWS SAM, Dockerfiles)
- Operational runbooks (startup procedures, deployment scripts, troubleshooting guides)

### Plan Construction

Build the plan following these principles:

#### Principle 1: Each Phase Produces a Working System

Every phase must end with a system that can be **started and tested**, even if incomplete. This means:
- Phase 1 always includes project scaffolding, build tooling, and a minimal running application
- Each subsequent phase adds capabilities while keeping the system functional
- No phase should leave the system in a broken state

#### Principle 2: Incremental Capability Delivery

Each phase delivers a coherent set of **new capabilities** — features that were not available in the previous phase. A capability maps to one or more Success Criteria checkboxes from the goals.

Define each phase with:
- **Name**: a short descriptive name (e.g., "Project Scaffolding & Dev Environment", "Core Data Pipeline", "User Dashboard")
- **Capabilities delivered**: a bulleted list of what becomes possible after this phase that was not possible before. Reference goal Success Criteria where applicable.
- **Tasks**: the ordered list of short tasks within this phase

#### Principle 3: Short Tasks

Each task should represent a **small, focused unit of work** — ideally completable by human developers in a single coding session. Guidelines:
- A task implements one feature, one endpoint, one component, or one piece of infrastructure
- If a task description contains "and" connecting two distinct pieces of work, split it
- Prefer many small tasks over few large ones
- Ideally, each task should be individually testable or verifiable

#### Principle 4: Must-Haves Before Should-Haves

Phases that deliver Must-have goal capabilities come before phases that deliver Should-have capabilities. Within a priority level, order by dependency graph.

#### Principle 5: Infrastructure and Integration Early

Cross-cutting infrastructure (scaffolding, build tooling, database schema, deployment configuration, unit and integration testing pipelines) should appear in early phases so that feature work can build on a solid foundation.

#### Principle 6: Deploy Readiness

If `4-deploy/` expectations exist, include infrastructure-as-code tasks (Terraform modules, SAM templates, Dockerfiles, deployment scripts) and operational runbook tasks at appropriate phases — not all at the end. A basic deployment mechanism should be available relatively early so the system can be tested in a realistic environment.

#### Principle 7: Manual Testing Readiness

Every phase must end with a **manual testing readiness task** (`TASK-phase-N-manual-testing`) that ensures a human can verify the delivered capabilities without reading source code. The task should:

1. **Create or update a deploy runbook** in `4-deploy/runbooks/` with startup instructions and step-by-step manual test scenarios for the phase's new capabilities. Update existing runbooks rather than creating duplicates.
2. **Document build/run/test commands** in the README (or equivalent existing doc) of every component modified in this phase or previous phases (e.g., `3-code/<component>/README.md`). Create or update convenience scripts where useful; for single-command operations, documenting the command is sufficient.

The Component column must list all components involved in the current phase and previous phases. Place these tasks in the `### Deploy & Operations` task table section, dependent on the phase's last functional task, with `-` in the Req column. List them last in the Execution Plan for their phase.

### Workflow

#### 1. Present the Implementation Plan

Present the complete plan to the user in this format:

```
## Implementation Plan

### Phase 1: [Name]

**Capabilities delivered:**
- [What becomes possible after this phase]
- [Reference GOAL-* Success Criteria where applicable]

**Tasks:**

| # | Task ID | Task | Component | Req |
|---|---------|------|-----------|-----|
| 1 | TASK-kebab-name | Short description | backend | REQ-F-* |
| 2 | TASK-kebab-name | Short description | frontend | REQ-F-* |
| 3 | TASK-phase-1-manual-testing | Create/update runbook and scripts for manual testing of phase capabilities | backend, frontend | - |

### Phase 2: [Name]
...
```

For each phase, explain briefly why these capabilities are grouped together and why this ordering makes sense.

After the plan, provide a **summary table**:

| Phase | Name | # Tasks | Goal Coverage |
|-------|------|---------|---------------|
| 1 | ... | N | GOAL-a, GOAL-b |
| 2 | ... | N | GOAL-c |
| ... | ... | ... | ... |

And a **coverage check**: list each approved goal and confirm which phase(s) address its Success Criteria. Flag any Success Criteria not covered by any task.

**Wait for user approval** before proceeding. The user may reorder phases, split or merge them, add or remove tasks, or change priorities.

#### 2. Populate tasks.md

After approval, write the approved plan into `3-code/tasks.md`:

**Task Table sections**: Create one section per component (matching the per-component directories in `3-code/`) plus `### Setup & Infrastructure` for cross-cutting tasks and `### Deploy & Operations` for IaC and runbooks. Place each task in the section matching its component.

For each task, create a row:

| ID | Task | Priority | Status | Req | Dependencies | Updated | Notes |
|----|------|----------|--------|-----|--------------|---------|-------|

- **ID**: `TASK-kebab-name` — descriptive, unique
- **Task**: short description (imperative form)
- **Priority**: derive from the goal priority (Must-have goal → P1, Should-have → P2, infrastructure/cross-cutting → P0 or P1 based on criticality)
- **Status**: `Todo`
- **Req**: link(s) to requirement file(s) this task implements, or `-` for infrastructure tasks. Example: `[REQ-F-search-by-name](../1-spec/requirements/REQ-F-search-by-name.md)`
- **Dependencies**: `TASK-*` IDs this task depends on, or `-`
- **Updated**: today's date (YYYY-MM-DD)
- **Notes**: empty or brief clarification

**Execution Plan section**: Replace the placeholder blocks with the approved phases:

```markdown
### Phase 1: [Name]

**Capabilities delivered:**
- [capability]

**Tasks:**
1. TASK-kebab-name
2. TASK-kebab-name

### Phase 2: [Name]
...
```

#### 3. Update Current State

After writing tasks, update the `### Current State` subsection in `CLAUDE.md` to record that the implementation plan has been created. Include:
- Number of phases
- Total number of tasks
- Date of plan creation

Example addition: "Implementation plan created (2026-03-12): N phases, M tasks covering all approved goals."

### Interaction Style

- Present the full plan at once — do not drip-feed phases one at a time.
- Be explicit about trade-offs: if a capability could go in phase N or N+1, explain why you placed it where you did.
- If a goal's Success Criteria cannot be fully covered by implementation tasks alone (e.g., requires manual verification, external resources), note this in the coverage check.
- After writing `tasks.md`, summarize what was written and ask if adjustments are needed.

### Rules

- **Read-before-write**: always read existing files before proposing changes.
- **Do not create source code**: this skill creates the plan only. Actual implementation happens when tasks are executed.
- **Do not modify design documents**: if a design gap is discovered during planning, surface it to the user and recommend running `/SDLC-design` to address it.
- **Do not modify objective artifacts**: if a requirement gap is discovered, surface it and recommend `/SDLC-elicit`.
- **Preserve existing content**: when updating `tasks.md`, preserve the Status Legend, Priority Legend, and How to Update sections. Replace only the Task Table sections and Execution Plan section.
- **User approval required**: present the full plan and wait for explicit approval before writing to `tasks.md`.
- **Task ID uniqueness**: every `TASK-*` ID must be unique. Use descriptive kebab-case names, not numbers.
- **Traceability**: every task implementing a requirement must link to it in the Req column. Tasks not linked to requirements (infrastructure, tooling) should use `-`.
