# DEC-003: Trail

> Companion to `DEC-003-typescript-verification.md`.
> AI agents read this only when evaluating whether the decision is still
> valid or when proposing a change or supersession.

## Alternatives considered

### Option A: Run lint only
- Pros: faster; single command; catches style and import issues.
- Cons: silent on type errors — type-unsafe code passes undetected.

### Option B: Run typecheck only
- Pros: catches type errors; produces compiler diagnostics.
- Cons: does not catch ESLint issues (unused imports, stylistic violations, problematic patterns).

### Option C: Run both lint and typecheck (chosen)
- Pros: full coverage — no class of static error is silently ignored.
- Cons: slightly slower; two commands to run instead of one.

### Option D: Merge into a single script (e.g. `npm run check`)
- Pros: single command; less friction.
- Cons: requires a wrapper script to be maintained per component; not universally present in all projects; adds an abstraction that may hide which tool failed.

## Reasoning

Option C was chosen because the two tools check orthogonal things and neither is a superset of the other. Omitting either produces a misleading "all clear". The cost (running two commands) is negligible compared to the cost of shipping type-unsafe or style-violating code. Option D was considered but rejected because it requires infrastructure that may not exist and adds an indirection layer that obscures failure attribution.

## Human involvement

**Type**: `human-argued`
**Notes**: Originally documented as an inline guideline in `3-code/CLAUDE.code.md` under the "TypeScript Verification" section. Migrated to DEC-003 dual-file format on 2026-02-22 to make it discoverable via the decisions index and enforceable by AI agents in the same way as other decisions.

## Changelog

| Date | Change | Involvement |
|------|--------|-------------|
| 2026-02-22 | Initial decision (extracted from CLAUDE.code.md inline instructions) | `human-argued` |
