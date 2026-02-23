# CLAUDE.objectives.md

Phase-specific instructions for the **Objectives** phase. Extends [../CLAUDE.md](../CLAUDE.md).

## Purpose

This phase defines **what** we're building and **why**. Focus on clarity, measurability, and alignment with stakeholder needs.

## Artifacts

| Artifact | Location | Purpose |
|----------|----------|---------|
| Stakeholders | [`stakeholders.md`](stakeholders.md) | Roles with interests and influence |
| Goals | [`goals/`](goals/) | High-level outcomes (GOAL-NNN) |
| User Stories | [`user-stories/`](user-stories/) | User-facing capabilities (US-NNN) |
| Requirements | [`requirements/`](requirements/) | Testable system requirements (REQ-NNN) |
| Assumptions | [`assumptions/`](assumptions/) | Beliefs taken as true but not verified (ASM-NNN) |
| Constraints | [`constraints/`](constraints/) | Hard limits on design and implementation (CON-NNN) |

Each directory holds individual files (one per item) plus a `_template.md`. When a new entry is elicited, create the file from the template and add a row to the appropriate index below.

**Index sync rule**: the index tables below duplicate key metadata from individual files. The individual file is the source of truth. When you modify an artifact, update its index row in the same operation.

### Artifact flow

```
                         ┌── Assumptions
Stakeholders → Goals ────┤
                         ├── Constraints
                         └── User Stories → Requirements → Design (phase 2)
```

---

## AI Guidelines

### Elicitation order

1. **Stakeholders** — identify all roles before writing anything else
2. **Goals** — high-level outcomes, one per stakeholder concern
3. **Assumptions** — surface beliefs early, while cheap to challenge
4. **Constraints** — document hard limits before they surprise you in design
5. **User Stories** — capabilities from the stakeholder perspective
6. **Requirements** — formal, testable, derived from user stories or goals

### Per-artifact guidance

**Stakeholders**: ask who uses, funds, operates, or is affected by the system. Record influence level honestly — it drives conflict resolution. Add entries to [`stakeholders.md`](stakeholders.md).

**Goals**: decompose vague ideas into concrete, measurable outcomes. Use MoSCoW priority consistently.

**User Stories**: use "As a [role], I want [capability], so that [benefit]." The role must be an existing stakeholder ID. Acceptance criteria at the story level are high-level; detailed criteria live in requirements.

**Requirements**: use clear, testable language (not "should be fast" — use "response time < 200ms at p95"). Choose the correct ID prefix. Set `Status: Draft` when first created; only a human marks `Approved`.

**ID prefixes**: `REQ-F` Functional, `REQ-PERF` Performance, `REQ-SEC` Security, `REQ-REL` Reliability, `REQ-USA` Usability, `REQ-MNT` Maintainability, `REQ-PORT` Portability, `REQ-SCA` Scalability, `REQ-COMP` Compliance.

**Assumptions**: always record the risk level (what happens if wrong?) and a verification plan when possible.

**Constraints**: consider technical (platforms, dependencies), business (budget, timeline, team size), and operational (hosting, compliance) categories.

### Conflict resolution

A conflict exists when two or more requirements cannot both be satisfied as stated.

**Never resolve a conflict silently.** Always surface it before acting.

1. **Identify**: note conflicting requirement IDs, source stakeholders, influence levels, and why they are incompatible.
2. **Ask the user**: present what makes them incompatible, stakeholders and influence levels, two or more resolution options, and a recommended option if one is clearly better.
3. **Wait for explicit approval** before modifying any file.
4. **Apply**: update affected `REQ-NNN.md` files and index rows. Update dependent user stories or goals if affected. Record a design decision if the resolution imposes a recurring constraint.
5. **Verify**: no artifacts remain in a conflicting state after resolution.

### Assumption invalidation

When an assumption is found to be wrong or no longer holds:

1. **Identify impact**: list all artifacts (requirements, user stories, design decisions) that depend on the invalidated assumption.
2. **Ask the user**: present the invalidated assumption, the affected artifacts, and proposed adjustments or alternatives.
3. **Wait for explicit approval** before modifying any file.
4. **Apply**: change the assumption's Status to `Invalidated`. Update or flag all dependent artifacts as directed.
5. **Verify**: no artifacts remain based on the invalidated assumption without acknowledgment.

### Artifact deprecation

When an artifact (user story, requirement, assumption, constraint) is no longer relevant:

1. Propose deprecation to the user with rationale and downstream impact.
2. Wait for explicit approval.
3. Change Status to `Deprecated` in the artifact file. Update its index row.
4. Check for dependent artifacts — flag any that reference the deprecated item.

---

## Linking to Other Phases

- Goals, user stories, and requirements are referenced in design documents (`2-design/`)
- Acceptance criteria inform test cases (`3-code/`)
- Constraints and assumptions influence infrastructure decisions (`4-deploy/`)

---

## Goals Index

| File | ID | Priority | Summary |
|------|----|----------|---------|
<!-- Add rows as goals are created -->

---

## User Stories Index

| File | ID | Role | Priority | Status | Summary |
|------|----|------|----------|--------|---------|
<!-- Add rows as user stories are created -->

---

## Requirements Index

| File | ID | Type | Priority | Status | Summary |
|------|----|------|----------|--------|---------|
<!-- Add rows as requirements are created -->

---

## Assumptions Index

| File | ID | Category | Status | Risk | Summary |
|------|----|----------|--------|------|---------|
<!-- Add rows as assumptions are created -->

---

## Constraints Index

| File | ID | Category | Status | Summary |
|------|----|----------|--------|---------|
<!-- Add rows as constraints are created -->
