# Skill: /trace

Trace an artifact through the traceability chain.

## Instructions

You are tracing an artifact's relationships across the full SDLC traceability chain.

### Setup

1. Read `CLAUDE.md` (root instructions).
2. Based on the artifact type, read the relevant phase instruction files and artifact files.

### Traceability Chain (from RATIONALE.md)

```
Stakeholders → Goals → User Stories → Requirements → Decisions → Tasks → Code → Tests
```

### Usage

The user provides an artifact ID (e.g., `GOAL-001`, `REQ-F001`, `TASK-003`, `DEC-001`, `US-002`, `STK-001`).

### Workflow

1. **Identify the artifact type** from the ID prefix:
   - `STK-` → Stakeholder (in `1-objectives/stakeholders.md`)
   - `GOAL-` → Goal (in `1-objectives/goals/`)
   - `US-` → User Story (in `1-objectives/user-stories/`)
   - `REQ-` → Requirement (in `1-objectives/requirements/`)
   - `ASM-` → Assumption (in `1-objectives/assumptions/`)
   - `CON-` → Constraint (in `1-objectives/constraints/`)
   - `DEC-` → Decision (in `2-design/decisions/`)
   - `TASK-` → Task (in `3-code/tasks.md`)

2. **Read the artifact file** to extract its relationships (Source, Source stakeholder, Related goal, Derived Requirements, Req column, etc.).

3. **Trace upstream** (why does this exist?):
   - Follow `Source`, `Source stakeholder`, `Related goal` links back toward Stakeholders.
   - For each upstream artifact, read it and continue tracing until you reach a Stakeholder.

4. **Trace downstream** (what depends on this?):
   - Search for references to this artifact ID across the project:
     - In other artifact files (requirements referencing a goal, user stories referencing a goal, etc.)
     - In the decisions index (decisions sourced from this requirement)
     - In `tasks.md` (tasks implementing this requirement)
     - In design documents (references to requirement IDs)
   - For each downstream artifact, note its ID, type, and status.

5. **Check for gaps**:
   - Upstream dead-ends (artifact has no source or stakeholder)
   - Downstream dead-ends (approved requirement with no tasks, task with no implementation)
   - Broken links (references to artifacts that don't exist)

### Report Format

```
Artifact: [ID] — [Title]
Status: [current status]

UPSTREAM (why does this exist?)
  STK-001 (Product Owner)
    └─ GOAL-001 (User Authentication)
        └─ US-001 (Login Capability)
            └─ REQ-F001 (Email Login) ← THIS ARTIFACT

DOWNSTREAM (what depends on this?)
  REQ-F001 (Email Login)
    ├─ DEC-002 (Auth Token Strategy)
    ├─ TASK-003 (Implement login endpoint) [Done]
    └─ TASK-004 (Implement login UI) [In Progress]

GAPS
  - None found (or list specific gaps)
```

### Rules

- This is a read-only operation — do not modify any files.
- Use grep/search to find all references to the artifact ID across the project, not just the links in the artifact file itself.
- Report broken links clearly — they indicate maintenance issues.
- If the artifact ID doesn't exist, tell the user and suggest similar IDs if found.
