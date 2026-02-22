# CLAUDE.code.md

Phase-specific instructions for the **Code** phase. Extends [../CLAUDE.md](../CLAUDE.md).

## Purpose

This phase contains the **implementation**. Focus on clean, tested, maintainable code.

---

## Development Decisions

Implementation-level decisions that ensure consistency across the codebase. Each decision is documented in a separate file under [`decisions/`](decisions/). See [`decisions/README.md`](decisions/README.md) for the template and conventions.

### Decisions Index

| ID | Title | Summary |
|----|-------|---------|
| *None yet* | | |

### AI Agent Instructions for Decisions

**Before starting a task**:
1. Scan the decisions index above
2. Identify any decisions relevant to the task
3. Read the full decision file(s) for applicable decisions
4. Apply the decisions during implementation

**After completing a task**:
1. Evaluate whether the task introduced a new pattern or convention that should be consistent across the codebase
2. If a non-obvious implementation choice was made that future code should replicate, propose a new development decision:
   - Create a new `DEC-NNN-short-name.md` file in `decisions/` following the template
   - Add the decision to the index table above
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
├── decisions/            # Development decisions (DEC-NNN files)
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

### Backend-Frontend API Contract Enforcement

<!-- TODO: Remove this section if not applicable, or customize for your project -->

The backend and frontend maintain **separate type definitions**. Every code change that touches the API boundary must go through the contract verification procedure below.

#### Contract Architecture

The backend defines **explicit response types** for every endpoint, derived from entity types:

```
entities (DB schema)  ->  response types (API contract)  ->  frontend types (mirror)
```

**Key pattern**: Response types strip internal fields and add computed fields. Mapper functions in services perform the conversion.

#### When Backend APIs Are Created or Modified

Any change to a handler, service return type, or response shape **must** trigger these steps:

1. **Define or update backend response types** — Add/update the explicit response type derived from the entity type. Strip internal fields, include computed fields.
2. **Create or update mapper function** — Ensure handlers use the mapper, never returning raw entities.
3. **Read frontend types** — Compare against the updated backend response types.
4. **Sync frontend types** — Update so field names, types, optionality, and enum values match exactly.
5. **Update frontend API client** — Verify endpoint paths, HTTP methods, request bodies, and return types are consistent.
6. **Check frontend consumers** — Search for usages of modified types. Verify components access correct fields.
7. **Run typecheck on both sides**.
8. **Update API design doc** — If the public API surface changed, update `02-design/api-design.md`.

#### Common Contract Pitfalls to Avoid

- **Returning raw entities**: Handlers must use mapper functions, never return raw internal entities.
- **List field naming**: Use named fields (`users`, `products`), never generic `items`.
- **Type precision**: Use union types (`'active' | 'inactive'`) in frontend, not `string`.
- **Status enum drift**: If backend adds a new status value, frontend must handle it.
- **Optional vs. required**: Keep optionality in sync between backend and frontend types.

### When Implementing Features
1. Review relevant design docs in `02-design/`
2. Check requirements in `01-objectives/`
3. Update `tasks.md` to track progress
4. Implement with tests
5. **Run the API contract verification procedure** if the feature touches any API boundary
6. Update design docs if implementation differs

### When Fixing Bugs
1. Write a failing test that reproduces the bug
2. Fix the bug
3. Verify the test passes
4. **If the bug was a contract mismatch**, run the full contract verification procedure
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
