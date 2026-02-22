# CLAUDE.code.md

Phase-specific instructions for the **Code** phase. Extends [../CLAUDE.md](../CLAUDE.md).

## Purpose

This phase contains the **implementation**. Focus on clean, tested, maintainable code.

---

## Decisions

All project decisions live in [`2-design/decisions/`](../2-design/decisions/). Each decision has two files linked by a shared name stem:

- **`DEC-NNN-short-name.md`** — active record: current state of the decision and enforcement rules. Read during normal task execution when trigger conditions are met.
- **`DEC-NNN-short-name.history.md`** — decision trail: alternatives considered, reasoning, human involvement, and changelog. Read only when evaluating whether a decision is still valid or when proposing a change.

**Naming convention**: `NNN` is a three-digit sequential number (`001`, `002`, ...); `short-name` is a kebab-case summary (`api-contract`, `error-format`).

**Templates**: use [`2-design/decisions/_template.md`](../2-design/decisions/_template.md) and [`2-design/decisions/_template.history.md`](../2-design/decisions/_template.history.md) as starting points for new decisions.

### Agent navigation rules

- **During normal task execution**: consult the phase-specific decisions index below → read only the `DEC-NNN.md` files for relevant decisions → apply enforcement.
- **When evaluating or changing a decision**: read `DEC-NNN.md` first, then `DEC-NNN.history.md` → propose changes → update both files → append to the changelog with involvement type.
- **Never modify** `*.history.md` files except to append to the changelog or to add alternatives when proposing a supersession.

### Human involvement vocabulary

| Value | Meaning |
|-------|---------|
| `human-decided` | Human made the decision; AI had no significant role |
| `human-argued` | Human proposed and argued the rationale explicitly |
| `collaborative` | Human and AI jointly reasoned through options |
| `ai-proposed/human-approved` | AI proposed; human explicitly approved |
| `ai-proposed/auto-accepted` | AI proposed and recorded without explicit human approval |

### Decisions relevant to the code phase

| ID | Title | Trigger |
|----|-------|---------|
| [DEC-001](../2-design/decisions/DEC-001-api-contract.md) | API Contract Design | When creating or modifying an endpoint handler, service return type, API client call, or consumer component |
| [DEC-002](../2-design/decisions/DEC-002-nodejs-version-management.md) | Node.js Version Management | When running any Node.js tooling command (`npm`, `npx`, `node`, `tsc`, etc.) in a directory with a `.nvmrc` file |

---

## Component Guidelines

<!-- TODO: Add an entry for each component/codebase in this project -->

Each component in this project must have a living description in this section. The description must:

- **Summarize the component's role** — what it does, what it owns, and how it fits into the overall system
- **Reflect the current design** — update it whenever the architecture, tech stack, or responsibility boundaries change
- **Be written for AI agents** — concise enough to be loaded in context, precise enough to guide implementation decisions

If a component's description drifts from the actual implementation, treat it as a bug: correct the description (and the decisions index if needed) before continuing work.

### Template

Copy this block for each component:

```
### <Component Name>

<One or two sentences describing what this component does and the role it plays in the system.>

| Aspect | Details |
|--------|---------|
| Runtime / Language | |
| Framework | |
| Key responsibilities | |
| Owned data / APIs | |
| Testing approach | |
```

---

## Build Commands

Scripts and commands for running, building, linting, type-checking, and testing each component are documented in that component's own codebase — typically in its `package.json`, `Makefile`, `README.md`, or equivalent. Check there first.

When invoking any command, also apply any active decisions that affect tooling (e.g., runtime version management — see the decisions index above).

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
- Adopt DRY coding principle
- Organize the functions of the codebase into independent modules when possible

### Testing Standards
- Write tests for new functionality
- Maintain test coverage for critical paths
- Use descriptive test names that explain the scenario

### Documentation in Code
- Add comments only where logic isn't self-evident
- Keep inline documentation concise
- Document public APIs and interfaces

### When Implementing Features
1. Locate the task in `tasks.md`; read every `REQ-NNN` listed in its `Req` column before writing any code
2. Review relevant design docs in `2-design/`
3. Update status to `In Progress` in `tasks.md`
4. Implement with tests
5. **Apply relevant decisions** from the index above — run the full enforcement procedure for any decision triggered by this feature
6. Update design docs if implementation differs
7. Update status to `Done` and move the row to Completed Tasks

### When Recording Decisions

When a significant pattern or constraint emerges during implementation:

1. Copy [`2-design/decisions/_template.md`](../2-design/decisions/_template.md) → `2-design/decisions/DEC-NNN-short-name.md` and fill in all fields.
2. Copy [`2-design/decisions/_template.history.md`](../2-design/decisions/_template.history.md) → `2-design/decisions/DEC-NNN-short-name.history.md` and fill in all fields.
3. If the decision has enforcement implications for the **code phase**, add it to the index above.
4. If the decision has enforcement implications for the **design or deploy phase**, also add it to the indexes in [`2-design/CLAUDE.design.md`](../2-design/CLAUDE.design.md) and/or [`4-deploy/CLAUDE.deploy.md`](../4-deploy/CLAUDE.deploy.md).

Common triggers for new decisions: error handling patterns, UI interaction patterns, data flow conventions, naming conventions, security patterns.

### When Fixing Bugs
1. Write a failing test that reproduces the bug
2. Fix the bug
3. Verify the test passes
4. **Check relevant decisions** in the index above — if the bug indicates a decision violation, apply the full enforcement procedure for that decision
5. Consider if this reveals a design issue
6. **Proactively check for the same bug elsewhere** — search the codebase for other locations affected by the same pattern. If found, **ask the user explicitly** whether to fix them too (do not silently skip or silently fix them).

---

## Task Tracking

All development tasks are tracked in [tasks.md](tasks.md).

---

## Linking to Other Phases

- Implementation should follow designs in `2-design/`
- Tests should verify requirements from `1-objectives/`
- Infrastructure code must be placed in `4-deploy/`
