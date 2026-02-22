# DEC-001: Trail

> Companion to `DEC-001-api-contract.md`.
> AI agents read this only when evaluating whether the decision is still
> valid or when proposing a change or supersession.

## Alternatives considered

### Option A: Share types from a common package
- Pros: single source of truth; compile-time enforcement.
- Cons: tight coupling between codebases; harder to version independently; not feasible across different languages or runtimes.

### Option B: Explicit contract rules with mirrored types
- Pros: works across any language or runtime pair; contract is visible and auditable; can be enforced via code review and AI-assisted verification.
- Cons: requires discipline; types must be kept in sync manually or via tooling.

## Reasoning

Option A is the strongest technical guarantee but introduces a hard coupling between codebases that is often impractical — especially in polyglot environments or when frontend and backend are versioned independently. Option B relies on process rather than compiler enforcement, but the explicit contract rules make the obligation clear to both human reviewers and AI agents. The audit trail (mapper functions, named response types) also makes deviations visible during code review.

## Human involvement

**Type**: `human-argued`
**Notes**: Decision originally recorded as ADR-001 on 2026-02-22. Migrated to DEC-001 dual-file format on 2026-02-22 as part of the unification of the decision record system.

## Changelog

| Date | Change | Involvement |
|------|--------|-------------|
| 2026-02-22 | Initial decision (as ADR-001) | `human-argued` |
| 2026-02-22 | Migrated to DEC-001 dual-file format; enforcement split into design-phase and code-phase checks | `ai-proposed/human-approved` |
