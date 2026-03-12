# Tasks

## Status Legend

| Symbol | Status |
|--------|--------|
| `Todo` | Not started |
| `In Progress` | Currently being worked on |
| `Blocked` | Waiting on a dependency or decision |
| `Done` | Completed |
| `Cancelled` | No longer needed |

## Priority Legend

| Priority | Meaning |
|----------|---------|
| `P0` | Infrastructure / cross-cutting — required before feature work |
| `P1` | Implements a Must-have goal |
| `P2` | Implements a Should-have goal |
| `P3` | Implements a Could-have goal |

## How to Update

- When starting a task, set Status to `In Progress` and update the `Updated` column.
- When finishing a task, set Status to `Done` and update the `Updated` column.
- When a task is blocked, set Status to `Blocked`, note the blocker in `Notes`, and update the `Updated` column.
- Do **not** reorder rows within a section — the Execution Plan defines execution order.
- Do **not** change task IDs after creation.
- When cancelling a task, set Status to `Cancelled`, note the reason in `Notes`, and update the `Updated` column.

---

## Task Table

<!-- Req column: links to requirements this task implements (comma-separated), or "-" if none. -->

### Setup & Infrastructure

| ID | Task | Priority | Status | Req | Dependencies | Updated | Notes |
|----|------|----------|--------|-----|--------------|---------|-------|

<!-- Add one section per component (matching per-component directories in 3-code/). -->
<!-- Example: ### Backend, ### Frontend, etc. -->

### Deploy & Operations

| ID | Task | Priority | Status | Req | Dependencies | Updated | Notes |
|----|------|----------|--------|-----|--------------|---------|-------|

---

## Execution Plan

Defines the order in which tasks should be executed. Tasks are grouped into phases; complete all tasks in a phase before moving to the next. Within a phase, execute tasks in the listed order. Each phase ends with a deployable or testable system.

<!-- Update this section whenever tasks are created, reordered, or cancelled. -->

### Phase 1: [Name]

**Capabilities delivered:**
- [What becomes possible after this phase]
- [Reference GOAL-* Success Criteria where applicable]

**Tasks:**
1. TASK-kebab-name
2. TASK-kebab-name

<!-- Add more phases as needed: ### Phase 2, ### Phase 3, ... -->
