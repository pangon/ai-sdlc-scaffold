# Skill: /implement

Execute a development task following the full implementation procedure.

## Instructions

You are implementing a development task from `3-code/tasks.md`, following the complete procedure defined in `CLAUDE.code.md`.

### Setup

1. Read `CLAUDE.md` (root instructions) and `3-code/CLAUDE.code.md` (phase instructions). Follow both throughout this session.
2. Read `3-code/tasks.md` to see all tasks and the execution plan.
3. Read the Decisions index in `CLAUDE.code.md`.

### Task Selection

If the user specifies a task ID (e.g., `TASK-003`), use that task. Otherwise:
1. Look at the Execution Plan in `tasks.md`.
2. Find the first task in the current block that has `Status: Todo` and whose dependencies are all `Done`.
3. Propose this task to the user and wait for confirmation.

### Implementation Procedure (from CLAUDE.code.md)

Follow these steps exactly:

1. **Read requirements**: read every `REQ-NNN` file listed in the task's `Req` column. Understand what the task must achieve.
2. **Review design**: read relevant design docs in `2-design/` (architecture, data model, API design).
3. **Check IaC**: if the task touches infrastructure as code, also read `4-deploy/CLAUDE.deploy.md` and follow its instructions alongside code phase instructions.
4. **Update status**: change the task to `In Progress` in `tasks.md`, set `Updated` to today.
5. **Check for design gaps**: if you anticipate a significant divergence from the design, stop and follow the design gap procedure in `CLAUDE.code.md` before coding.
6. **Check decisions**: load and apply all relevant decisions from the index whose trigger conditions match.
7. **Implement with tests**: write the implementation and tests following the Code Quality and Testing guidelines in `CLAUDE.code.md`.
8. **Evaluate divergence**: after implementing, check if any divergence from the design occurred. Follow the design gap procedure if needed.
9. **Update status**: change the task to `Done` in `tasks.md`, set `Updated` to today.
10. **Update artifact status**: if all tasks linked to a requirement are now `Done`, update that requirement's Status to `Implemented`.

### Code Quality Rules (from CLAUDE.code.md)

- Write clear, self-documenting code following language/framework conventions
- Keep functions small and focused
- Use strict type checking where available
- DRY principle
- Organize into independent modules
- Prefer multiple files over single large files
- Comments only where logic isn't self-evident

### Design Gap Handling

**Minor divergence** (field renamed, type made more specific, optional field added): update the relevant `2-design/` file silently, continue.

**Significant divergence** (new endpoint/entity, invalid architectural assumption, requirement that cannot be implemented as designed):
1. Stop — do not write implementation code.
2. Surface the gap to the user.
3. Present options: update design first, simplify to stay within design, or accept and record deviation as a decision.
4. Wait for explicit approval before proceeding.

### Rules

- Always read requirements before writing code. Never implement from memory or assumption.
- Follow all active decisions whose trigger conditions match.
- Run tests after implementation to verify correctness (use build commands from the component's own codebase).
- One task at a time. Complete it fully before moving to the next.
