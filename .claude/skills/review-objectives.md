# Skill: /review-objectives

Dashboard and review for all Objectives phase artifacts.

## Instructions

You are generating a status review of the Objectives phase.

### Setup

1. Read `CLAUDE.md` (root instructions) and `1-objectives/CLAUDE.objectives.md` (phase instructions).
2. Read `1-objectives/stakeholders.md`.
3. Read all index tables in `CLAUDE.objectives.md` (Goals, User Stories, Requirements, Assumptions, Constraints).
4. For each index entry, read the individual artifact file to verify the index is in sync.

### Report

Generate a structured report with these sections:

#### 1. Stakeholders
- List all stakeholders with ID, role, and influence level.
- Flag if no stakeholders are defined yet.

#### 2. Artifact Summary
For each artifact type (Goals, User Stories, Requirements, Assumptions, Constraints):
- Total count
- Breakdown by status (Draft / Approved / Implemented / Deprecated)
- Breakdown by priority (for Goals, User Stories, Requirements)

#### 3. Traceability Check
- Goals without linked user stories or requirements
- User stories without derived requirements
- Requirements without a source (user story or goal)
- Requirements without a source stakeholder
- Orphaned artifacts (not linked to anything upstream)

#### 4. Conflicts and Issues
- Any requirements that conflict with each other
- Assumptions with high risk that are still Unverified
- Index rows that are out of sync with their artifact files

#### 5. Phase Gate Readiness (Objectives → Design)
Check the preconditions from `CLAUDE.md`:
- [ ] At least one goal Approved
- [ ] At least one requirement Approved
- [ ] Stakeholders defined

State clearly whether the gate is met or what is missing.

### Rules

- This is a read-only operation — do not modify any files.
- Present findings clearly and concisely.
- If issues are found, suggest specific actions the user can take.
