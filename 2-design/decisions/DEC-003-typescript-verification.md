# DEC-003: TypeScript Verification

**Status**: Active
**Category**: Convention
**Scope**: system-wide
**Source**: n/a
**Last updated**: 2026-02-22

## Context

TypeScript projects typically expose two separate tools for static analysis: ESLint (style, patterns, unused imports) and the TypeScript compiler `tsc` (type correctness). Running only one tool produces a false sense of safety — ESLint passes silently when type errors exist, and `tsc` does not catch stylistic or import issues. AI agents that run only `lint` after writing TypeScript code may ship type-unsafe code without noticing.

## Decision

After writing or modifying TypeScript code, always run both linters in sequence:

1. `npm run lint` — ESLint check
2. `npm run typecheck` — TypeScript compiler check (`tsc --noEmit` or equivalent)

Both must pass before the task is considered complete.

## Enforcement

### Trigger conditions

- **Code phase**: after writing or modifying any TypeScript (`.ts`, `.tsx`) file in a component that exposes `lint` and `typecheck` scripts.

### Required patterns

```bash
# Always run both — order does not matter, but both must succeed
npm run lint
npm run typecheck
```

If the project uses the nvm prefix (DEC-002), apply it to both commands:

```bash
source ~/.nvm/nvm.sh && nvm use && npm run lint && npm run typecheck
```

### Required checks

After any TypeScript file is written or modified:

1. Identify the component's package root (the directory containing `package.json`).
2. Verify that both `lint` and `typecheck` scripts are defined in `package.json`.
3. Run `npm run lint`; fix any reported issues before continuing.
4. Run `npm run typecheck`; fix any reported type errors before continuing.
5. Do not mark the task complete until both commands exit with code 0.

### Prohibited patterns

- Running only `npm run lint` after writing TypeScript code.
- Running only `npm run typecheck` after writing TypeScript code.
- Skipping verification entirely (e.g., assuming the code is correct by inspection).
