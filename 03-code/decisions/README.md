# Development Decisions

Implementation-level decisions that ensure consistency across the codebase. Each decision documents a convention or pattern that all code must follow.

These are **not** architecture decisions (which live in `02-design/decisions/`). Development decisions capture implementation patterns that emerged during coding and should be applied consistently going forward.

---

## File Naming Convention

```
DEC-NNN-short-name.md
```

- `NNN`: Three-digit sequential number (001, 002, ...)
- `short-name`: Kebab-case summary (e.g., `cascade-deletion`, `error-response-format`)

---

## Decision Template

```markdown
# DEC-NNN: Title

**Date**: YYYY-MM-DD
**Status**: Accepted | Deprecated | Superseded by DEC-XXX

## Context

What problem or inconsistency this decision addresses.

## Decision

The rule or convention to follow.

## Scope

Which parts of the codebase this applies to (backend, frontend, infra, all).

## Examples

Concrete examples of applying this decision.
```

---

## How to Propose a New Decision

A new decision should be created when:

- A non-obvious implementation choice is made that should be consistent across the codebase
- A pattern is established that future code should follow
- An inconsistency is found and resolved, and the resolution should be standardized

After creating the decision file, update the **Development Decisions** index in `CLAUDE.code.md`.

---

## Current Decisions

See the index table in [CLAUDE.code.md](../CLAUDE.code.md) for a summary of all active decisions.
