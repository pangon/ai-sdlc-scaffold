# Skill: /decide

Record, review, deprecate, or supersede design decisions.

## Instructions

You are managing design decisions following the procedures defined in the project's SDLC scaffold.

### Setup

1. Read `CLAUDE.md` (root instructions) — especially the Decisions section. Follow it throughout this session.
2. Read `2-design/CLAUDE.design.md` (phase instructions) for the current decisions index.
3. Read `2-design/decisions/_template.md` and `2-design/decisions/_template.history.md` to understand the file format.

### Operations

Ask the user what they want to do:

#### A. Record a New Decision

1. Discuss the decision context with the user: what problem or situation makes this decision necessary.
2. Explore alternatives — present at least two options with pros and cons.
3. Once the user chooses (or you propose and the user approves), create both files:
   - `2-design/decisions/DEC-NNN-<short-name>.md` from `_template.md` — fill in all fields
   - `2-design/decisions/DEC-NNN-<short-name>.history.md` from `_template.history.md` — fill in all fields
4. Determine the correct `NNN` by checking existing decision files for the next sequential number.
5. Set the appropriate human involvement type (see vocabulary in `CLAUDE.md`).
6. Add the decision to the index table in every `CLAUDE.<phase>.md` file whose trigger conditions are met.

#### B. Review an Existing Decision

1. Ask which decision to review (by ID or keyword).
2. Read both `DEC-NNN.md` and `DEC-NNN.history.md`.
3. Present a summary: context, decision, enforcement rules, alternatives considered, and changelog.
4. Ask the user if any changes are needed.

#### C. Deprecate a Decision

Follow the deprecation procedure from `CLAUDE.md` exactly:
1. Read both files for full context.
2. Present to the user: why it's no longer valid, what depends on it, the proposed action.
3. Wait for explicit approval.
4. Change Status to `Deprecated`, append to changelog, remove from all phase indexes.

#### D. Supersede a Decision

Follow the supersession procedure from `CLAUDE.md` exactly:
1. Read both files of the old decision.
2. Present the case for supersession.
3. Wait for explicit approval.
4. Create the new decision (DEC-MMM) using operation A.
5. Update old decision: Status to `Superseded by DEC-MMM`, append to changelog.
6. Replace old rows with new ones in all phase indexes.

### Rules

- Never modify `*.history.md` except to append to the changelog or add alternatives during supersession.
- Use kebab-case for file names: `DEC-002-api-versioning.md`.
- Every decision must have trigger conditions defining when agents should check it.
- Deprecation and supersession always require explicit human approval — this is an "always ask" tier action.
