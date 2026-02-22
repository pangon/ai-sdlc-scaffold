# Decision Records

This directory contains all project decisions — architecture choices, design conventions, coding patterns, process rules — regardless of the phase in which they were made.

---

## Structure

Each decision consists of exactly two files, linked by a shared name stem:

```
DEC-NNN-short-name.md          ← active record
DEC-NNN-short-name.history.md  ← decision trail
```

### Active record (`DEC-NNN-short-name.md`)

The operational file. Contains the current state of the decision and the enforcement rules an AI agent must apply. Agents read this when the trigger conditions listed in the phase index (`CLAUDE.design.md` or `CLAUDE.code.md`) are met.

### Trail (`DEC-NNN-short-name.history.md`)

The rationale file. Contains alternatives considered, reasoning, human involvement classification, and a dated changelog. Agents read this only when evaluating whether a decision is still valid or when proposing a change or supersession.

---

## Naming convention

- `NNN`: three-digit sequential number (`001`, `002`, ...)
- `short-name`: kebab-case summary (`api-contract`, `error-format`)

---

## Active record template

```markdown
# DEC-NNN: [Title]

**Status**: Active | Deprecated | Superseded by DEC-XXX
**Category**: Architecture | Convention | Process | Data
**Scope**: system-wide | backend | frontend | infra | [codebase-name]
**Last updated**: YYYY-MM-DD

## Context

What situation, problem, or constraint makes this decision necessary.
What would go wrong without a recorded decision here.

## Decision

Authoritative statement of what was decided. Concise and imperative.
This is the source of truth; the trail file explains the reasoning.

## Enforcement

> Written for AI agents. Defines what must be checked, produced, or verified
> whenever this decision is relevant. Omit this section for decisions that
> have no procedural implications (e.g., a one-time technology choice with
> no recurring verification steps).

### Trigger conditions

Explicit conditions that activate this decision. If the decision applies to multiple phases,
list one trigger per phase:

- **Design phase**: when ...
- **Code phase**: when ...

These are the authoritative triggers. Phase indexes (`CLAUDE.design.md`, `CLAUDE.code.md`)
reference them, possibly rephrased to fit the phase context, but this file is the source
of truth.

### Required patterns

Concrete rules to follow. Include code snippets or structural examples
where precision matters.

### Required checks

Step-by-step verification procedure, designed to be executed mechanically.

1. Check X
2. Verify Y
3. Confirm Z

### Prohibited patterns

Anti-patterns that violate this decision. Be explicit — agents need to
recognize these when reading existing code.
```

---

## Trail template

```markdown
# DEC-NNN: Trail

> Companion to `DEC-NNN-[name].md`.
> AI agents read this only when evaluating whether the decision is still
> valid or when proposing a change or supersession.

## Alternatives considered

### Option A: [Name]
- Pros: ...
- Cons: ...

### Option B: [Name]
- Pros: ...
- Cons: ...

## Reasoning

What tipped the balance toward the chosen option.
Trade-offs explicitly accepted. Conditions that would invalidate this reasoning.

## Human involvement

**Type**: [see vocabulary below]
**Notes**: [optional — who was involved, any dissent, caveats]

## Changelog

| Date | Change | Involvement |
|------|--------|-------------|
| YYYY-MM-DD | Initial decision | ... |
```

---

## Human involvement vocabulary

| Value | Meaning |
|-------|---------|
| `human-decided` | Human made the decision; AI had no significant role |
| `human-argued` | Human proposed and argued the rationale explicitly |
| `collaborative` | Human and AI jointly reasoned through options |
| `ai-proposed/human-approved` | AI proposed; human explicitly approved |
| `ai-proposed/auto-accepted` | AI proposed and recorded without explicit human approval |

---

## Agent navigation rules

- **During normal task execution**: consult the phase-specific decisions index in `CLAUDE.design.md` or `CLAUDE.code.md` → read only the `DEC-NNN.md` files for relevant decisions → apply enforcement.
- **When evaluating or changing a decision**: read `DEC-NNN.md` first, then `DEC-NNN.history.md` → propose changes → update both files → append to the changelog with involvement type.
- **Never modify** `*.history.md` files except to append to the changelog or to add alternatives when proposing a supersession.
