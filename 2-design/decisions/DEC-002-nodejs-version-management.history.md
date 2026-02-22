# DEC-002: Trail

> Companion to `DEC-002-nodejs-version-management.md`.
> AI agents read this only when evaluating whether the decision is still
> valid or when proposing a change or supersession.

## Alternatives considered

### Option A: Trust the system Node.js
- Pros: no prefix required; simpler commands.
- Cons: system Node.js version may be incompatible; errors are silent and hard to diagnose.

### Option B: Explicit nvm prefix on every command
- Pros: guarantees the correct Node.js version; works reliably in non-interactive shells.
- Cons: more verbose commands.

### Option C: Shell hook (e.g. direnv, nvm auto-use)
- Pros: transparent — no prefix needed once configured.
- Cons: requires additional tooling not guaranteed to be present; does not work in the non-interactive shell used by AI agents.

## Reasoning

Option B was chosen because it is reliable across all shell types without requiring additional tooling. The prefix is explicit and auditable, which matters when AI agents compose commands — there is no ambiguity about which Node.js version is in use.

## Human involvement

**Type**: `human-argued`
**Notes**: Originally documented as an inline commented example in `CLAUDE.md` under the "Shell Environment" section. Migrated to DEC-002 dual-file format on 2026-02-22 as part of the unification of the decision record system.

## Changelog

| Date | Change | Involvement |
|------|--------|-------------|
| 2026-02-22 | Initial decision (extracted from CLAUDE.md inline instructions) | `human-argued` |
| 2026-02-22 | Extended scope to deploy phase: added deploy trigger condition and added entry to decisions index in `4-deploy/CLAUDE.deploy.md` | `ai-proposed/human-approved` |
