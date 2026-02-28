# Skill: /plan-tasks

Generate development tasks from approved requirements and design.

## Instructions

You are creating or updating the task backlog in `3-code/tasks.md` based on approved requirements and design documents.

### Setup

1. Read `CLAUDE.md` (root instructions) and `3-code/CLAUDE.code.md` (phase instructions). Follow both throughout this session.
2. Read the Decisions index in `CLAUDE.code.md` and load any relevant decisions.
3. Read `3-code/tasks.md` to understand existing tasks and the ID sequence.
4. Read the Requirements index in `1-objectives/CLAUDE.objectives.md` — focus on `Approved` requirements.
5. Read the design documents in `2-design/` (architecture, data model, API design) to understand implementation context.

### Workflow

1. **Identify approved requirements** that do not yet have corresponding tasks. Cross-reference the `Req` column in existing tasks against approved requirement IDs.

2. **For each uncovered requirement**, propose one or more tasks:
   - Write a clear, actionable task description
   - Assign the next sequential `TASK-NNN` ID
   - Set the `Req` column with the requirement ID(s) this task implements
   - Propose a priority (P0–P3) based on the requirement's priority
   - Set `Status: Todo`
   - Identify dependencies on other tasks if applicable
   - Set `Updated` to today's date

3. **Present the proposed tasks** to the user as a table before writing. Include:
   - Total new tasks proposed
   - Which requirements each task covers
   - Dependency relationships
   - Suggested execution order

4. **After user approval**, update `3-code/tasks.md`:
   - Add new task rows to the appropriate section (create new sections if needed, e.g., `### Backend`, `### Frontend`)
   - Update the Execution Plan section: group tasks into blocks with a logical order

5. **Verify** that every `Approved` requirement has at least one corresponding task after the update.

### Task Decomposition Guidelines

- One task should be completable in a single implementation session.
- If a requirement needs multiple tasks, make dependencies explicit.
- Infrastructure/setup tasks come first (Block 1).
- Prefer small, focused tasks over large, multi-concern tasks.
- Include test-writing as part of each task, not as separate tasks (per `CLAUDE.code.md` guidelines).

### Rules

- Never create tasks for `Draft` requirements — only `Approved`.
- Every task must link to at least one requirement in the `Req` column (or `-` for pure infrastructure tasks).
- Follow the task format defined in `3-code/tasks.md` (ID, Task, Priority, Status, Req, Dependencies, Updated, Notes).
- Do not start implementing — this skill only plans the backlog.
