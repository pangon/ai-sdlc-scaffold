---
name: SDLC-status
description: Project-wide status dashboard across all SDLC phases. Use when the user wants an overview of the entire project state. Aggregates artifact counts, statuses, and phase gate readiness into a single report.
---

## Instructions

You are generating a comprehensive status report for the entire SDLC project.

### Setup

1. Read all four phase instruction files:
   - `1-objectives/CLAUDE.objectives.md`
   - `2-design/CLAUDE.design.md`
   - `3-code/CLAUDE.code.md`
   - `4-deploy/CLAUDE.deploy.md`
2. Read `1-objectives/stakeholders.md`.
3. Read `3-code/tasks.md`.
4. Scan all artifact indexes in the phase files.
5. List files in `2-design/decisions/` to count decisions (exclude templates).
6. List files in `4-deploy/infrastructure/`, `4-deploy/scripts/`, `4-deploy/runbooks/` to count deployment artifacts.

### Report Structure

#### Project Overview
- Project name (from `CLAUDE.md` overview section)
- Current date

#### Phase 1: Objectives
| Artifact | Total | Draft | Approved | Implemented | Deprecated |
|----------|-------|-------|----------|-------------|------------|
| Goals | | | | | |
| User Stories | | | | | |
| Requirements | | | | | |
| Assumptions (Unverified/Verified/Invalidated) | | | | | |
| Constraints (Active/Lifted) | | | | | |

#### Phase 2: Design
- Architecture document: empty / in-progress / complete
- Data model: empty / in-progress / complete
- API design: empty / in-progress / complete
- Decisions: N active, N deprecated, N superseded

#### Phase 3: Code
| Status | Count |
|--------|-------|
| Todo | |
| In Progress | |
| Blocked | |
| Done | |
| Cancelled | |

- Requirements coverage: N of M approved requirements have corresponding tasks

#### Phase 4: Deploy
- Infrastructure files: count
- Scripts: count
- Runbooks: count

#### Phase Gates
For each transition, state whether preconditions are met.

#### Suggestions
Based on the current state, suggest the most impactful next actions (e.g., "Approve draft requirements to unblock Design phase", "Create tasks for approved requirements").

### Rules

- This is a read-only operation — do not modify any files.
- Use checkmarks and crosses for gate readiness.
- Keep the report concise — summary numbers, not full artifact listings.
- If a phase has no artifacts at all, note it but don't alarm — the project may simply not be at that phase yet.
