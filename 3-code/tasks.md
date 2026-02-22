# Development Tasks

This document tracks all development tasks for the project. Update this file as tasks are completed or new tasks are identified.

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
| P0 | Critical — blocks everything |
| P1 | High — needed for MVP |
| P2 | Medium — important but not blocking |
| P3 | Low — nice to have |

---

## Open Tasks

<!-- Organize tasks by domain or component -->
<!-- Req column: list all REQ-NNN IDs this task implements, comma-separated. Use "-" if the task has no direct requirement (e.g. tooling, scaffolding). -->

### Setup & Infrastructure

| ID | Task | Priority | Status | Req | Dependencies | Notes |
|----|------|----------|--------|-----|--------------|-------|
| TASK-001 | Initial project setup | P0 | Todo | - | - | - |

<!-- ### Backend -->
<!-- ### Frontend -->
<!-- ### Testing -->
<!-- ### Documentation -->

---

## Completed Tasks

<!-- Move completed tasks here with completion date -->

| ID | Task | Req | Completed Date | Notes |
|----|------|-----|----------------|-------|
| - | - | - | - | - |

---

## Task Execution Order

The following is the recommended order for executing tasks, respecting all dependencies. Add as many stages as needed. Each stage groups tasks that can be worked on once the previous stage is complete.

<!-- EXAMPLE — replace with your actual stages and tasks -->

### Stage 1: Example Stage Name

| Order | Task ID | Description | Depends On |
|-------|---------|-------------|------------|
| 1 | TASK-001 | Example first task | - |
| 2 | TASK-002 | Example second task | TASK-001 |

### Stage 2: Another Example Stage

| Order | Task ID | Description | Depends On |
|-------|---------|-------------|------------|
| 3 | TASK-003 | Example task in stage 2 | TASK-002 |

<!-- Add more stages as needed: Stage 3, Stage 4, ... -->

---

## Notes

### How to Update This Document
When creating a task:
1. Assign the next sequential `TASK-NNN` ID
2. Fill in the `Req` column with all `REQ-NNN` IDs this task implements (comma-separated); use `-` only if no requirement applies
3. Set status to `Todo`

When starting a task:
1. Change status from `Todo` to `In Progress`
2. Read all requirements listed in the `Req` column before writing any code

When completing a task:
1. Change status to `Done`
2. Move the row to "Completed Tasks" section
3. Add completion date and any notes

When blocked:
1. Change status to `Blocked`
2. Add note explaining the blocker
3. Create a new task if needed to resolve the blocker
