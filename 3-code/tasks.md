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
| P0 | Critical — blocks everything |
| P1 | High — needed for MVP |
| P2 | Medium — important but not blocking |
| P3 | Low — nice to have |

---

## Task Table

<!-- Req column: links to requirements this task implements (comma-separated), or "-" if none. -->
<!-- Example Req entry: [REQ-F-search-by-name](../1-objectives/requirements/REQ-F-search-by-name.md) -->
<!-- Updated column: date of the last status change (YYYY-MM-DD). -->

### Setup & Infrastructure

| ID | Task | Priority | Status | Req | Dependencies | Updated | Notes |
|----|------|----------|--------|-----|--------------|---------|-------|

<!-- Add sections per component: ### Backend, ### Frontend, etc. -->

---

## Execution Plan

Defines the order in which tasks should be executed. Group tasks into blocks; complete all tasks in a block before moving to the next. Within a block, execute tasks in the listed order.

<!-- Update this section whenever tasks are created, reordered, or cancelled. -->

### Block 1: [Name]

1. TASK-kebab-name
2. TASK-kebab-name

<!-- Add more blocks as needed: ### Block 2, ### Block 3, ... -->

---

## How to Update

- **Create**: choose a descriptive `TASK-kebab-name` ID, fill `Req` column with links to requirement files, set `Todo`.
- **Start**: change to `In Progress`, set `Updated` to today, read all linked requirements first.
- **Complete**: change to `Done`, set `Updated` to today.
- **Block**: change to `Blocked`, set `Updated`, add note explaining the blocker.
- **Cancel**: change to `Cancelled`, set `Updated`, add note explaining why.
