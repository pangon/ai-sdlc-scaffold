# DEC-NNN: [Title]

**Status**: Active | Deprecated | Superseded by DEC-XXX
**Category**: Architecture | Convention | Process | Data
**Scope**: system-wide | backend | frontend | infra | [codebase-name]
**Source**: [REQ-NNN](../../1-objectives/requirements/REQ-NNN-*.md), [GOAL-NNN](../../1-objectives/goals/GOAL-NNN-*.md), [CON-NNN](../../1-objectives/constraints/CON-NNN-*.md), or n/a
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
- **Deploy phase**: when ...

These are the authoritative triggers. Phase indexes (`CLAUDE.design.md`, `CLAUDE.code.md`, `CLAUDE.deploy.md`)
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
