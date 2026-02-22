# CLAUDE.objectives.md

Phase-specific instructions for the **Objectives** phase. Extends [../CLAUDE.md](../CLAUDE.md).

## Purpose

This phase defines **what** we're building and **why**. Focus on clarity, measurability, and alignment with stakeholder needs.

## Artifact Overview

| Artifact | Location | Purpose |
|----------|----------|---------|
| Stakeholders | [Stakeholders section below](#stakeholders) | Roles with interests and influence — the authority behind all requirements |
| Goals | [`goals/`](goals/) | High-level outcomes the project must achieve (GOAL-NNN) |
| User Stories | [`user-stories/`](user-stories/) | User-facing capabilities, expressed from a stakeholder perspective (US-NNN) |
| Requirements | [`requirements/`](requirements/) | Formal, testable system requirements derived from user stories (REQ-NNN) |
| Assumptions | [`assumptions/`](assumptions/) | Beliefs taken as true but not yet verified (ASM-NNN) |
| Constraints | [`constraints/`](constraints/) | Hard limits on design and implementation (CON-NNN) |

Each directory holds individual files (one per item) plus a `_template.md`. When the human elicits a new entry through conversation, the AI agent must create the file by following the template, then add a row to the appropriate index table below.

### Artifact flow

```
                         ┌── Assumptions  (beliefs held while pursuing the goals)
Stakeholders → Goals ────┤
                         ├── Constraints  (hard limits imposed alongside the goals)
                         │
                         └── User Stories → Requirements → Design (phase 2)
```

---

## AI Guidelines for This Phase

### Recommended elicitation order

Work through artifacts in this order to build a coherent, traceable requirements baseline:

1. **Stakeholders** — identify all roles before writing anything else
2. **Goals** — capture high-level outcomes, one per stakeholder concern
3. **Assumptions** — surface beliefs early, while they are cheap to challenge
4. **Constraints** — document hard limits before they surprise you in design
5. **User Stories** — elicit capabilities from the stakeholder perspective
6. **Requirements** — derive formal, testable requirements from user stories

### When identifying stakeholders
- Ask: who uses the system? who funds it? who is affected by it? who operates it?
- Record influence level honestly — it drives conflict resolution
- Every requirement should trace to at least one stakeholder
- Add new stakeholders directly to the **Stakeholders** table below

### When defining goals
- Ask clarifying questions to understand the user's vision
- Decompose vague ideas into concrete, measurable outcomes
- Identify potential conflicts between goals early
- Use MoSCoW priority consistently across all artifacts
- After creating a goal file, add a row to the **Goals Index** below

### When writing user stories
- Use the standard format: "As a [role], I want [capability], so that [benefit]"
- The role must be an existing stakeholder ID from the **Stakeholders** table below
- Acceptance criteria at the story level are high-level — detailed criteria live in requirements
- After creating a user story, add a row to the **User Stories Index** below

### When writing requirements
- Requirements are derived from user stories (or directly from goals if no story layer is used)
- Use clear, testable language — avoid "should be fast" → use "response time < 200ms at p95"
- Choose the correct ID prefix (REQ-F, REQ-PERF, REQ-SEC, …) — it signals design impact
- Set `Status: Draft` when first created; mark `Approved` after stakeholder review
- After creating a requirement, add a row to the **Requirements Index** below

### When documenting assumptions
- An assumption is something believed true but not yet verified — distinct from a constraint
- Always record the risk level: what happens if this assumption is wrong?
- Provide a verification plan when possible
- After creating an assumption file, add a row to the **Assumptions Index** below

### When documenting constraints
- Consider technical constraints (platforms, languages, dependencies)
- Consider business constraints (budget, timeline, team size)
- Consider operational constraints (hosting, maintenance, compliance)
- After creating a constraint file, add a row to the **Constraints Index** below

---

## Linking to Other Phases

- Goals, user stories, and requirements are referenced in design documents (`2-design/`)
- Acceptance criteria from requirements inform test cases (`3-code/<codebase>/tests/`)
- Constraints and assumptions influence infrastructure decisions (`4-deploy/`)
- Invalidated assumptions should trigger a review of dependent requirements and design decisions

---

## Stakeholders

Everyone with a stake in the system: those who use it, fund it, maintain it, or are affected by it. Stakeholders are the authority behind goals, user stories, and requirements — every requirement should ultimately trace back to a stakeholder need.

**Influence levels**:
- **High** — can approve or veto decisions; priority conflicts are resolved in their favor
- **Medium** — consulted during requirements review; their concerns are addressed but may be overruled
- **Low** — informed of decisions; their needs are considered but not blocking

When a priority conflict arises between requirements, resolve it by checking influence levels and which stakeholders are the source of each conflicting requirement.

| ID | Role | Description | Interests | Influence |
|----|------|-------------|-----------|-----------|
| STK-001 | Placeholder Role | Replace with your first stakeholder role | What they care about | High / Medium / Low |

---

## Goals Index

> **Agent navigation**: scan this table to identify which goal files are relevant to your task. Read only the files you need — do not open all of them.

| File | ID | Priority | Summary |
|------|----|----------|---------|
| [GOAL-001-placeholder.md](goals/GOAL-001-placeholder.md) | GOAL-001 | Must-have | Placeholder — replace with your first project goal |

---

## User Stories Index

> **Agent navigation**: scan this table to identify which user stories are relevant to your task. The Role column helps narrow down stories by stakeholder.

| File | ID | Role | Priority | Summary |
|------|----|------|----------|---------|
| [US-001-placeholder.md](user-stories/US-001-placeholder.md) | US-001 | [Role] | Must-have | Placeholder — replace with your first user story |

---

## Requirements Index

> **Agent navigation**: scan this table to identify which requirement files are relevant to your task. The Type column distinguishes functional from non-functional entries and their subcategory.
>
> **ID prefixes**: `REQ-F` Functional · `REQ-PERF` Performance · `REQ-SEC` Security · `REQ-REL` Reliability · `REQ-USA` Usability · `REQ-MNT` Maintainability · `REQ-PORT` Portability · `REQ-SCA` Scalability · `REQ-COMP` Compliance

| File | ID | Type | Status | Priority | Summary |
|------|----|------|--------|----------|---------|
| [REQ-F001-placeholder.md](requirements/REQ-F001-placeholder.md) | REQ-F001 | Functional | Draft | Must-have | Placeholder functional requirement |
| [REQ-PERF001-placeholder.md](requirements/REQ-PERF001-placeholder.md) | REQ-PERF001 | Performance | Draft | Must-have | Placeholder performance requirement |

---

## Assumptions Index

> **Agent navigation**: scan this table before making design or implementation decisions — an invalidated assumption may change what is possible.

| File | ID | Category | Status | Risk | Summary |
|------|----|----------|--------|------|---------|
| [ASM-001-placeholder.md](assumptions/ASM-001-placeholder.md) | ASM-001 | — | Unverified | High | Placeholder — replace with your first assumption |

---

## Constraints Index

> **Agent navigation**: scan this table to identify which constraint files are relevant to your task. The Category column distinguishes technical, business, and operational entries.

| File | ID | Category | Summary |
|------|----|----------|---------|
| [CON-001-technical.md](constraints/CON-001-technical.md) | CON-001 | Technical | Placeholder technical constraint |
| [CON-002-business.md](constraints/CON-002-business.md) | CON-002 | Business | Placeholder business constraint |
| [CON-003-operational.md](constraints/CON-003-operational.md) | CON-003 | Operational | Placeholder operational constraint |
