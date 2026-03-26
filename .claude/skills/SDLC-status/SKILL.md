---
name: SDLC-status
description: Project-wide status dashboard across all SDLC phases. Use when the user wants an overview of the entire project state. Aggregates artifact counts, statuses, phase gate readiness, traceability health, and assessment freshness into a single report.
---

## Instructions

You are generating a comprehensive status report for the entire SDLC project.

### Setup

1. Read root instructions: `CLAUDE.md` — extract Project Overview and Current State.
2. Read all four phase instruction files:
   - `1-spec/CLAUDE.spec.md`
   - `2-design/CLAUDE.design.md`
   - `3-code/CLAUDE.code.md`
   - `4-deploy/CLAUDE.deploy.md`
3. Read `1-spec/stakeholders.md`.
4. Read `3-code/tasks.md` (if it exists and has content).
5. Scan all artifact indexes in the phase files — follow File column links only when needed to resolve ambiguous statuses.
6. List files in `decisions/` to count decisions (exclude `PROCEDURES.md`, templates, and `.history.md` files; read `.history.md` only when the corresponding active record shows Deprecated or Superseded status).
7. List component directories in `3-code/` (directories containing `CLAUDE.component.md`).
8. List files in `4-deploy/infrastructure/`, `4-deploy/scripts/`, `4-deploy/runbooks/` to count deployment artifacts (exclude templates).

### Phase Validation

Before generating the report, determine project state:
- If `CLAUDE.md` Project Overview still contains placeholder text → report project as **Not Initialized** and recommend `/SDLC-init`.
- Otherwise → proceed with full report.

### Report Structure

Present the report as a single markdown document with these sections.

---

#### Project Overview
- **Project**: name/description (from `CLAUDE.md` Project Overview)
- **Date**: current date
- **Current State**: reproduce the Current State section from `CLAUDE.md` verbatim — this is the authoritative project-level summary maintained by all skills

---

#### Phase 1: Specification

**Stakeholders**: N defined (list names and influence levels in a compact line)

**Artifacts**:

| Artifact | Total | Draft | Approved | Implemented | Deprecated |
|----------|-------|-------|----------|-------------|------------|
| Goals | | | | | |
| User Stories | | | | | |
| Requirements | | | | | |

| Artifact | Total | Unverified | Verified | Invalidated |
|----------|-------|------------|----------|-------------|
| Assumptions | | | | |

| Artifact | Total | Active | Lifted |
|----------|-------|--------|--------|
| Constraints | | | |

If requirements exist, add a **breakdown by class** (only classes that have at least one artifact):

| Class | Total | Draft | Approved | Implemented |
|-------|-------|-------|----------|-------------|
| REQ-F | | | | |
| REQ-SEC | | | | |
| ... | | | | |

**Gap Analysis**: report the last recorded gap analysis from the Specification Current State — date, severity counts, and whether it is **fresh** or **stale** (stale = artifacts modified since last analysis). If no gap analysis has been recorded, state "Not performed".

---

#### Phase 2: Design

**Documents**:

| Document | Status |
|----------|--------|
| Architecture (`architecture.md`) | empty / in-progress / complete |
| Data Model (`data-model.md`) | empty / in-progress / complete |
| API Design (`api-design.md`) | empty / in-progress / complete |

Determine status: **empty** if file contains only headings/placeholders; **complete** if all major sections have substantive content; **in-progress** otherwise.

**Decisions**: N active, N deprecated, N superseded

**Completeness Assessment**: report the last recorded assessment from the Design Current State — date, severity counts, and whether it is **fresh** or **stale**. If no assessment has been recorded, state "Not performed".

---

#### Phase 3: Code

**Components**: list identified components (from `3-code/` directories with `CLAUDE.component.md`), showing name and technology. If no components → state "Not decomposed yet".

**Task Summary** (if `tasks.md` exists and has tasks):

| Status | Count |
|--------|-------|
| Todo | |
| In Progress | |
| Blocked | |
| Done | |
| Cancelled | |
| **Total** | |

**Execution Plan Progress** (if execution plan exists in `tasks.md`):
- Current phase: the earliest phase with incomplete tasks (not all Done/Cancelled)
- Phase progress: for each phase, show `Done/Total` count and phase name
- Overall: `N/M tasks done (X%)`

**Requirements Coverage**: N of M approved requirements have at least one linked task (scan Req column in task table). List any approved requirements without tasks.

---

#### Phase 4: Deploy

| Artifact Type | Count |
|---------------|-------|
| Infrastructure files | |
| Scripts | |
| Runbooks | |

---

#### Traceability Health

Perform a lightweight traceability scan (read index tables, not every file):

- **Orphaned goals**: goals not linked to any user story
- **Orphaned user stories**: user stories not linked to any requirement
- **Stakeholders without goals**: stakeholders with no associated goals
- **Uncovered requirement classes**: requirement classes (of the 9 defined) with zero artifacts
- **Requirements without tasks**: approved requirements not referenced in any task's Req column

Report counts only. If all checks pass, state "No traceability issues detected".

---

#### Phase Gates

**Spec → Design**:
| Precondition | Status |
|--------------|--------|
| Stakeholders defined | ✅ / ❌ |
| At least one goal Approved | ✅ / ❌ |
| At least one requirement Approved | ✅ / ❌ |
| Gap analysis recorded, fresh, no Critical gaps | ✅ / ❌ / ⚠️ stale |

**Design → Code**:
| Precondition | Status |
|--------------|--------|
| Architecture drafted | ✅ / ❌ |
| Data Model drafted | ✅ / ❌ |
| API Design drafted | ✅ / ❌ |
| Completeness assessment recorded, fresh, no Critical findings | ✅ / ❌ / ⚠️ stale |
| Components identified (per-component directories in `3-code/`) | ✅ / ❌ |

Use ✅ when met, ❌ when not met, ⚠️ when partially met or stale.

---

#### Suggested Next Actions

Based on the current state, suggest **up to 5** concrete next actions ordered by impact. Tailor suggestions to the project's actual phase:

- If not initialized → recommend `/SDLC-init`
- If in Specification → recommend elicitation actions, gap analysis, approvals
- If at Specification → Design gate → recommend resolving gate blockers
- If in Design → recommend completing documents, recording decisions, running completeness assessment
- If at Design → Code gate → recommend resolving gate blockers, running `/SDLC-decompose`
- If in Code → recommend next task execution, resolving blocked tasks, addressing traceability gaps
- If tasks complete → recommend deploy activities, final review

Each suggestion should reference the specific skill or action (e.g., "Run `/SDLC-elicit` to perform gap analysis before advancing to Design").

### Rules

- **Read-only operation** — do not modify any files.
- **Concise** — summary numbers and compact tables, not full artifact listings. The goal is a dashboard, not a dump.
- **No phase is alarming if empty** — the project may simply not be at that phase yet. State it neutrally.
- **Skip empty sections gracefully** — if a phase has no artifacts, show a single line (e.g., "No specification artifacts yet") instead of empty tables.
- **Assessment freshness matters** — always report whether gap analysis and completeness assessments are fresh or stale, as this directly impacts phase gate readiness.
