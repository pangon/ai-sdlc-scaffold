## Language Policy

**All AI outputs must be in English**, regardless of the language used in user prompts. This applies to code, comments, documentation, configuration files, commit messages, and response text.

## Memory Policy

**Do not use Claude memory files to store project information**. All project knowledge — domain context, team structure, constraints, decisions, and any other relevant information — must be captured exclusively through the SDLC artifact system (stakeholders, constraints, assumptions, goals, requirements, decisions, etc.). This ensures all knowledge is structured, traceable, and available to every team member working on the project.

---

## Project Overview

<!-- Replace this section with a description of your project. -->

This repository uses a structured, AI-first development lifecycle. All project knowledge — specification, design, decisions, tasks — lives alongside the source code.

### Current State

**Phase**: Not initialized

**Summary**: Base scaffold, not yet initialized. The repository contains the AI SDLC framework (phase directories, templates, automation skills), ready to be populated starting from the Specification phase after initialization (`/SDLC-init`).

---

## Current State Protocol

The `### Current State` subsection above is the machine-readable project status shared by all skills. Its structure:

- **`**Phase**:`** — mandatory first line. One of: `Not initialized`, `Specification`, `Design`, `Code`. Every skill validates its applicability against this field before acting.
- **`**Summary**:`** — mandatory. One or two sentences of free text describing where the project stands.
- **Standardized status lines** — each optional, appearing at most once, added when its activity first occurs, then updated in place; never removed:

| Line | Format | Primary maintainer |
|------|--------|--------------------|
| `**Spec artifacts**:` | artifact types worked on so far | SDLC-elicit |
| `**Gap analysis**:` | active or passed form — see Assessment lines below | SDLC-elicit |
| `**Design documents**:` | drafting status per document | SDLC-design |
| `**Completeness assessment**:` | active or passed form — see Assessment lines below | SDLC-design |
| `**Components**:` | comma-separated component names | SDLC-decompose |
| `**Implementation plan**:` | `created YYYY-MM-DD — N phases, M tasks` | SDLC-implementation-plan |
| `**Task progress**:` | `D/M tasks done — currently in Phase N (name)` | SDLC-execute-task |

**Assessment lines** — `**Gap analysis**:` and `**Completeness assessment**:` record the run date, a `fresh`/`stale (reason)` marker, and the compact list of open issues: `YYYY-MM-DD — fresh — open: <Severity>: <short summary>; …` (or `— no issues` when the run found none). An issue is removed from the list when a change corrects it; when the last one goes, the tail becomes `all issues resolved`. A new run overwrites the whole line. When the corresponding phase gate is passed, the issue list is removed, keeping the date and how the gate was passed: `passed: no issues`, `passed: all issues resolved`, or `passed: user accepted remaining issues (<counts by severity>)` — `not performed — passed: user accepted to proceed` if no run was recorded. A line with a `fresh`/`stale` marker is in **active form**; after the gate it is in **passed form**.

Maintenance rules — they apply to **every** artifact change, whether performed inside a skill or not:

1. **Same-operation update**: update the affected lines in the same operation as the artifact change they reflect.
2. **Staleness** (active-form assessment lines only): changes to Specification artifacts flip `**Gap analysis**:` to `stale (artifacts changed since)`; changes to design documents or decisions flip `**Completeness assessment**:` to `stale (design changed since)`, and Specification changes flip it too (`stale (spec changed since)`). Exception: a change that only corrects listed open issues updates the list instead, and status-only lifecycle advancements (e.g., `Approved → Implemented`) do not flip the marker.
3. **Phase transitions**: `**Phase**:` changes only when a phase gate is crossed (see Phase Gates); the crossed gate's assessment line switches to its passed form.

---

## Phase-Specific Instructions

Each phase directory contains a `CLAUDE.<phase>.md` file. When working in a phase:

1. Read the phase-specific instructions — they extend (not override) this file
2. Consult the decisions index in that phase file before starting work (for the Code phase, decisions indexes are in each component's own `CLAUDE.md` file — `3-code/<component-name>/CLAUDE.md` — not in `CLAUDE.code.md`)
3. Work within the appropriate phase structure

| Phase | Directory | Focus |
|-------|-----------|-------|
| **Specification** | `1-spec/` | Define what to build and why |
| **Design** | `2-design/` | Define how to build it |
| **Code** | `3-code/` | Build it |
| **Deploy** | `4-deploy/` | Ship and operate it |

### Cross-Skill Artifact Procedures

Any modification to phase artifacts — whether performed inside a skill, during a free-prompt conversation, or as a side effect of any other task — must follow the authoritative procedures for that phase:

- **Specification artifacts** (`1-spec/`): follow the procedures in [`.claude/skills/SDLC-elicit/SKILL.md`](.claude/skills/SDLC-elicit/SKILL.md) — including traceability rules, status downgrade on modification, index synchronization, bidirectional link maintenance, and Current State tracking.
- **Design artifacts** (`2-design/`): follow the procedures in [`.claude/skills/SDLC-design/SKILL.md`](.claude/skills/SDLC-design/SKILL.md) — including downstream effect checks, decision recording triggers, requirement coverage verification, and Current State tracking.
- **Code phase task artifacts** (`3-code/tasks.md`): for creating or re-planning tasks, follow the procedures in [`.claude/skills/SDLC-implementation-plan/SKILL.md`](.claude/skills/SDLC-implementation-plan/SKILL.md) — including phased task grouping, traceability links, and incremental deployability. For task status transitions, follow [`.claude/skills/SDLC-execute-task/SKILL.md`](.claude/skills/SDLC-execute-task/SKILL.md) — including task list integrity, status propagation to Specification artifacts, and Current State tracking.
- **Component source code** (`3-code/<component-name>/`): follow the procedures in [`.claude/skills/SDLC-fix/SKILL.md`](.claude/skills/SDLC-fix/SKILL.md) — or [`.claude/skills/SDLC-execute-task/SKILL.md`](.claude/skills/SDLC-execute-task/SKILL.md) when the change executes a planned task — including decisions application, tests, design-gap handling, and implementation logging.
- **Deploy artifacts** (`4-deploy/`): follow the AI Guidelines and index rules in [`4-deploy/CLAUDE.deploy.md`](4-deploy/CLAUDE.deploy.md), through the same skill procedures as component source code (`SDLC-execute-task` for planned tasks, `SDLC-fix` for ad-hoc changes).

### Phase Gates

Before creating artifacts in the next phase, check these minimum preconditions. **This table is the single source of truth for gate preconditions** — skills evaluate it directly and must not define their own variants.

| Transition | Precondition | How to verify |
|------------|--------------|---------------|
| Spec → Design | Stakeholders defined | At least one real (non-placeholder) row in `1-spec/stakeholders.md` |
| Spec → Design | At least one requirement Approved | Requirements index in `1-spec/CLAUDE.spec.md` |
| Spec → Design | Gap analysis fresh, no Critical gaps | `**Gap analysis**:` line in Current State: present in active form, `fresh`, no open Critical issues |
| Design → Code | All design documents drafted | `architecture.md`, `data-model.md`, `api-design.md` all have real content |
| Design → Code | Completeness assessment fresh, no Critical findings | `**Completeness assessment**:` line in Current State: present in active form, `fresh`, no open Critical findings |
| Design → Code | Components identified | Per-component directories (each with a `CLAUDE.md`) in `3-code/` |

**Crossing a gate always requires explicit user confirmation** — phase gate advancement is an "always ask" action (see Graduated Safeguards). When all preconditions are met, ask the user to confirm the advancement; when some are not, list them and proceed only if the user explicitly accepts the gaps. The crossing is recorded by updating `**Phase**:` in Current State: `/SDLC-design` sets `Design` when the first user-approved design change is applied; `/SDLC-decompose` sets `Code` when the component directories are created (which also satisfies the "components identified" precondition).

There is no gate between Code and Deploy. Deploy activities (deployments, runbooks, infrastructure setup) can happen at any time during the Code phase.

The `/SDLC-validate` skill mechanically verifies these preconditions and the underlying artifact invariants (index synchronization, link resolution, status coherence, assessment freshness against git history).

---

## Artifacts

All project knowledge is captured as structured markdown files alongside the source code. This gives AI agents the full context that human developers would normally carry in their heads or scattered across external tools, and creates a traceability chain from business goals to deployed code.

### Types and locations

| Prefix | Artifact | Location |
|--------|----------|----------|
| `GOAL` | Goals | `1-spec/goals/` |
| `US` | User Stories | `1-spec/user-stories/` |
| `REQ-CLASS` | Requirements | `1-spec/requirements/` |
| `ASM` | Assumptions | `1-spec/assumptions/` |
| `CON` | Constraints | `1-spec/constraints/` |
| `STK` | Stakeholders | `1-spec/stakeholders.md` (rows) |
| `TASK` | Tasks | `3-code/tasks.md` (rows) |
| `DEC` | Decisions | `decisions/` |

### Naming

All artifact IDs use the pattern `PREFIX-kebab-name` — a type prefix followed by a descriptive kebab-case name. The descriptive name **is** the unique identifier (e.g., `DEC-use-postgres`, `REQ-F-search-by-name`). There are no numeric sequences, to avoid ID collisions when working on parallel branches.

### Phase indexes

Every `CLAUDE.<phase>.md` file contains index tables listing the artifacts in that phase. Each index must include a **File column** with a relative link to the artifact file, so that AI agents can discover the file name and human reviewers can navigate easily.

---

## Graduated Safeguards

AI agents operate autonomously within development tasks. For project-level decisions, the scaffold defines three tiers:

| Tier | When | Agent behavior |
|------|------|----------------|
| **Always ask** | Conflict resolution, design gaps, decision deprecation/supersession, phase gate advancement | Stop, present options, wait for human approval |
| **Ask first time, then follow precedent** | Naming conventions, error handling patterns, test structure | Ask once, record the decision, apply consistently afterward |
| **Decide and record** | Routine implementation choices within established patterns | Decide autonomously, record in the appropriate artifact |

When spotting a related issue, potential improvement, or ambiguous situation during a task, **surface it to the user** instead of silently deciding to act or not act.

---

## Decisions

Decisions live in `decisions/`. Each decision has two files:

- **`DEC-kebab-name.md`** — the active record (context, decision, enforcement). Read during normal task execution.
- **`DEC-kebab-name.history.md`** — the trail (alternatives, reasoning, changelog). Read only when evaluating or changing a decision.

Each `CLAUDE.<phase>.md` contains a decisions index with trigger conditions. A decision may appear in multiple phase indexes.

### How to use decisions during tasks

1. Consult the decisions index in the current phase's `CLAUDE.<phase>.md`, or in the component's `CLAUDE.md` (`3-code/<component-name>/CLAUDE.md`) when working within a specific component.
2. Follow the File column link to read the relevant `DEC-*.md` file.
3. Apply its enforcement rules.

Do **not** modify `*.history.md` except to append to the changelog.

### Recording, deprecating, or superseding decisions

When a significant decision, pattern, or constraint emerges, record it as a new decision. For the recording procedure, as well as deprecation and supersession, see [`decisions/PROCEDURES.md`](decisions/PROCEDURES.md).

---

## After Making Changes

Evaluate whether to:

1. **Update this file** if project-wide patterns or architecture change significantly.
2. **Update phase-specific files** (`CLAUDE.<phase>.md`) if phase-specific patterns or conventions are established.
3. **Create new instruction files** if a workflow becomes complex enough to need dedicated guidance.

Proactively suggest these updates when relevant.
