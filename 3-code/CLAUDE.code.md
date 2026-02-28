# CLAUDE.code.md

Phase-specific instructions for the **Code** phase. Extends [../CLAUDE.md](../CLAUDE.md).

## Purpose

This phase contains the **implementation**. Focus on clean, tested, maintainable code.

---

## Decisions Relevant to This Phase

<!-- Add rows as decisions are recorded. -->

| ID | Title | Trigger |
|----|-------|---------|
| [DEC-001](../2-design/decisions/DEC-001-error-format.md) | Error Response Format | When implementing error handling or returning errors from any component |

---

## Component Guidelines

<!-- Add an entry for each component/codebase. Copy the template block below. -->

Each component must have a brief description here. Update it when architecture or responsibilities change. If a description drifts from the implementation, correct it before continuing work.

```
### <Component Name>

<One or two sentences describing what this component does.>

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

Scripts and commands for each component are documented in that component's own codebase (package.json, Makefile, README, or equivalent). Check there first.

When invoking any command, apply active decisions from the index above whose trigger conditions match.

---

## AI Guidelines

### Code Quality
- Write clear, self-documenting code following language/framework conventions
- Keep functions small and focused
- Use strict type checking where available
- Adopt DRY coding principle
- Organize into independent modules when possible
- Prefer splitting code across multiple files over keeping a single large file, when compatible with language/framework conventions
- Add comments only where logic isn't self-evident
- Keep inline documentation concise

### Testing
- Write tests for new functionality
- Maintain test coverage for critical paths
- Use descriptive test names that explain the scenario

### Implementing Features
1. Locate the task in [`tasks.md`](tasks.md); read every `REQ-NNN` listed in its `Req` column before writing code
2. Review relevant design docs in `2-design/`
3. If the task touches infrastructure as code (Terraform, CloudFormation, Dockerfiles, CI/CD pipelines, Helm charts, etc.), also read [`4-deploy/CLAUDE.deploy.md`](../4-deploy/CLAUDE.deploy.md) and follow its instructions alongside these
4. Update status to `In Progress` in `tasks.md`
5. If you anticipate a significant divergence from the design, **stop and follow the [design gap procedure](#design-gaps)** before coding
6. Check relevant decisions from the index above
7. Implement with tests
8. After implementing: evaluate whether any divergence from the design occurred and follow the design gap procedure if needed
9. Update status to `Done` in `tasks.md`

### Design Gaps

A design gap is any divergence between design documents and what implementation requires.

**Minor divergence** (field renamed, type made more specific, optional field added): update the relevant `2-design/` file silently, continue.

**Significant divergence** (new endpoint/entity, invalid architectural assumption, requirement that cannot be implemented as designed):

1. **Stop** — do not write implementation code.
2. **Surface** the gap: what the design says, what implementation needs, why they differ.
3. **Present options**: update design first, simplify to stay within design, or accept and record deviation as a decision.
4. **Wait for explicit approval** before proceeding.
5. **Act**: update `2-design/` or `1-objectives/` as needed, then implement.

### Fixing Bugs
1. Write a failing test that reproduces the bug
2. Fix the bug and verify the test passes
3. Check relevant decisions — if the bug indicates a violation, apply the full enforcement procedure
4. Search the codebase for the same pattern elsewhere. If found, **ask the user** whether to fix them too.

### Common Decision Triggers
When a significant decision emerges, follow [CLAUDE.md — Decisions](../CLAUDE.md#when-recording-decisions). Common triggers: error handling patterns, data flow conventions, naming conventions, security patterns.

---

## Task Tracking

All development tasks are tracked in [`tasks.md`](tasks.md).

---

## Linking to Other Phases

- Implementation follows designs in `2-design/`
- Tests verify requirements from `1-objectives/`
- Infrastructure code goes in `4-deploy/`; when a coding task modifies IaC, the deploy phase instructions ([`CLAUDE.deploy.md`](../4-deploy/CLAUDE.deploy.md)) apply as well
