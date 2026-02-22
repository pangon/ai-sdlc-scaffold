- Keep documentation close to the code it describes


# CLAUDE.code.md

Phase-specific instructions for the **Code** phase. Extends [../CLAUDE.md](../CLAUDE.md).

## Purpose

This phase contains the **implementation**. Focus on clean, tested, maintainable code.

---

## Decisions

All project decisions live in [`02-design/decisions/`](../02-design/decisions/). Each decision has two files: `DEC-NNN-name.md` (active record) and `DEC-NNN-name.history.md` (trail). See [`02-design/decisions/README.md`](../02-design/decisions/README.md) for the full format specification.

### Decisions relevant to the code phase

| ID | Title | Trigger |
|----|-------|---------|
| [DEC-001](../02-design/decisions/DEC-001-api-contract.md) | API Contract Design | When creating or modifying an endpoint handler, service return type, API client call, or consumer component |
| [DEC-002](../02-design/decisions/DEC-002-nodejs-version-management.md) | Node.js Version Management | When running any Node.js tooling command (`npm`, `npx`, `node`, `tsc`, etc.) in a directory with a `.nvmrc` file |

### AI Agent Instructions

**Before starting a task**:
1. Scan the decisions index above
2. Read the `DEC-NNN.md` file for each relevant decision
3. Apply the enforcement procedures during implementation

**After completing a task**:
1. Evaluate whether the task introduced a pattern or constraint that should be consistent across the codebase
2. If yes, propose a new decision:
   - Create `02-design/decisions/DEC-NNN-short-name.md` and `DEC-NNN-short-name.history.md` following the templates in [`02-design/decisions/README.md`](../02-design/decisions/README.md)
   - Add it to the relevant phase indexes (`CLAUDE.design.md`, `CLAUDE.code.md`, or both)
3. Common triggers for new decisions: error handling patterns, UI interaction patterns, data flow conventions, naming conventions, security patterns

---

## Key File Paths

<!-- TODO: Fill in with your project's key files -->

Quick reference for frequently accessed files:

| File | Path |
|------|------|
| Task Tracker | `03-code/tasks.md` |
<!-- | Frontend package.json | `03-code/frontend/package.json` | -->
<!-- | Backend package.json | `03-code/backend/package.json` | -->

---

## Directory Structure

<!-- TODO: Customize for your project's actual structure -->

```
03-code/
├── CLAUDE.code.md        # This file
├── tasks.md              # Development tasks tracking
│
├── src/                  # Source code
│   ├── ...               # Organize by feature, layer, or domain
│   └── shared/           # Shared utilities, types, constants
│
└── tests/                # Test suites
```

---

## Component Guidelines

<!-- TODO: Customize for your tech stack -->
<!-- Example sections for common setups: -->

<!--
### Frontend

| Aspect | Details |
|--------|---------|
| Framework | React / Vue / Svelte / etc. |
| UI Library | MUI / Tailwind / etc. |
| State | Context / Redux / Zustand / etc. |
| Build Tool | Vite / webpack / etc. |
| Testing | Vitest / Jest / etc. |

### Backend

| Aspect | Details |
|--------|---------|
| Runtime | Node.js / Python / Go / etc. |
| Language | TypeScript / Python / etc. |
| Framework | Express / FastAPI / etc. |
| Testing | Jest / pytest / etc. |
-->

---

## Build Commands

<!-- TODO: Fill in your actual build commands -->
<!-- IMPORTANT: If using nvm, prefix all npm/npx/node commands with: -->
<!-- source ~/.nvm/nvm.sh && nvm use && -->

```bash
# Development server
# npm run dev

# Run tests
# npm test

# Lint
# npm run lint

# Type check (if applicable)
# npm run typecheck

# Build for production
# npm run build
```

---

## AI Guidelines for This Phase

### TypeScript Verification (if applicable)

**Always run BOTH lint AND typecheck after writing TypeScript code:**

ESLint and TypeScript compiler check different things:
- **`npm run lint`** -> code style, unused imports, problematic patterns
- **`npm run typecheck`** -> type compatibility, assignment errors, structural types

Running only `lint` will miss TypeScript compiler errors. Always run both.

### Code Quality Standards
- Write clear, self-documenting code
- Follow language/framework conventions
- Keep functions small and focused
- Handle errors appropriately
- Use strict type checking where available

### Testing Standards
- Write tests for new functionality
- Maintain test coverage for critical paths
- Use descriptive test names that explain the scenario

### Documentation in Code
- Add comments only where logic isn't self-evident
- Keep inline documentation concise
- Document public APIs and interfaces

### When Implementing Features
1. Review relevant design docs in `02-design/`
2. Check requirements in `01-objectives/`
3. Update `tasks.md` to track progress
4. Implement with tests
5. **Apply relevant decisions** from the index above — run the full enforcement procedure for any decision triggered by this feature
6. Update design docs if implementation differs

### When Fixing Bugs
1. Write a failing test that reproduces the bug
2. Fix the bug
3. Verify the test passes
4. **Check relevant decisions** in the index above — if the bug indicates a decision violation, apply the full enforcement procedure for that decision
5. Consider if this reveals a design issue
6. **Proactively check for the same bug elsewhere** — search the codebase for other locations affected by the same pattern. If found, **ask the user explicitly** whether to fix them too (do not silently skip or silently fix them).

---

## Task Tracking

All development tasks are tracked in [tasks.md](tasks.md). Before starting work:

1. Check `tasks.md` for current task status
2. Pick the highest-priority unblocked task
3. Update status to `In Progress`
4. Complete the task
5. Update status to `Done`

See [tasks.md](tasks.md) for the complete task list and tracking format.

---

## Linking to Other Phases

- Implementation should follow designs in `02-design/`
- Tests should verify requirements from `01-objectives/`
- Infrastructure code here, deployment scripts in `04-deploy/`
