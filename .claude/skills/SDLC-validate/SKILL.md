---
name: SDLC-validate
description: Read-only consistency validation across all SDLC artifacts. Verifies the structural invariants the skills maintain — index/file synchronization, link resolution, status legality and coherence, ID uniqueness, traceability orphans, and Current State accuracy including git-verified assessment freshness. Reports findings with suggested corrections; changes nothing. Use before phase gates, after manual edits, or periodically.
---

## Instructions

You are running a read-only consistency validation over the repository's SDLC artifacts. You verify mechanically checkable invariants and report violations — you do **not** modify any file. This skill is the deep counterpart of `/SDLC-status`: where the dashboard reads index tables and trusts them, validation opens the artifacts and verifies that the indexes, links, statuses, and Current State tell the truth.

### Setup

1. Read `CLAUDE.md` — the Current State Protocol, the `### Current State` content, the Phase Gates table, and the artifact naming rules. These are the authorities the checks verify against; do not invent stricter rules than documented.
2. Read `1-spec/CLAUDE.spec.md` (index tables, status lifecycles), `2-design/CLAUDE.design.md`, `3-code/CLAUDE.code.md`, and `4-deploy/CLAUDE.deploy.md`.
3. Read `1-spec/stakeholders.md`.
4. List artifact files, excluding `_template*.md` files: `1-spec/goals/`, `1-spec/user-stories/`, `1-spec/requirements/`, `1-spec/assumptions/`, `1-spec/constraints/`, `decisions/` (also excluding `PROCEDURES.md`), `3-code/*/CLAUDE.md` (component directories), `3-code/tasks.md`, `3-code/implementation-log/`, `4-deploy/runbooks/`.
5. Open artifact files as required by the checks below.

### Checks

Run every check whose subject exists; skip checks whose subject does not exist yet (e.g., task checks before an implementation plan) and list the skips in the report — a phase not yet reached is not a finding.

#### 1. Index ↔ file synchronization

- Every artifact file has exactly one row in its index; every index row's File link resolves to an existing file.
- Index metadata (Type, Status, Priority, link columns) matches the fields inside the artifact file.
- Decision indexes: every `DEC-*.md` with a matching trigger appears in the corresponding phase/component index; no index lists a decision whose Status is `Deprecated` or `Superseded`.

#### 2. Link resolution

- Every artifact-to-artifact reference resolves to an existing file: `Source`, `Source stakeholder`, `Related goal`, Derived Requirements, Related Artifacts sections.
- Referenced stakeholder IDs exist as rows in `stakeholders.md`.
- `tasks.md`: Req column links resolve to requirement files; Dependencies reference existing task IDs.
- Component `CLAUDE.md` files: rows in `## Requirements Addressed` and `## Relevant Decisions` resolve.

#### 3. IDs and naming

- Every ID follows `PREFIX-kebab-name` with the correct prefix for its location, and the file name matches the ID in the title.
- No duplicate artifact IDs across files; no duplicate task IDs in `tasks.md`.

#### 4. Status legality and coherence

- Every Status value belongs to its artifact's lifecycle vocabulary (per `1-spec/CLAUDE.spec.md`, the decision template, and the `tasks.md` legends).
- Requirement `Implemented` ⇔ all its linked tasks are `Done` (ignoring `Cancelled` and `Decomposed`, at least one `Done`). Both directions are findings: `Implemented` with pending tasks is an illegal state; `Approved` with all tasks `Done` is a missed propagation (see `/SDLC-execute-task`).
- User story `Implemented` ⇒ all its derived requirements are `Implemented`. Goal `Achieved` ⇒ all its Success Criteria checkboxes are ticked.
- Task `Blocked`, `Cancelled`, or `Decomposed` ⇒ Notes column non-empty (for `Decomposed`, listing the subtask IDs).
- At most one task `In Progress` — more than one indicates interrupted runs that were never reconciled.
- Design documents: the `**Status**:` field is one of `Stub`, `Draft`, `Approved (date)`, `N/A (reason)`; `Stub` documents carry no substantive content, while `Draft`/`Approved` documents do.

#### 5. Traceability orphans

- Stakeholders not referenced by any goal's Stakeholder column.
- Goals not referenced by any user story's Goal column; user stories not referenced by any requirement's Source column; requirements with an empty or unresolvable Source.
- In the Code phase: `Approved` requirements not referenced by any task's Req column.

#### 6. Current State accuracy

- `**Phase**:` holds a legal value; the mandatory lines exist; status lines follow the protocol formats.
- `**Components**:` matches the directories in `3-code/` that contain a `CLAUDE.md`.
- `**Implementation plan**:` and `**Task progress**:` counts match `tasks.md` (totals, Done count, current phase).
- **Assessment freshness (git-verified)** — for each assessment line in active form claiming `fresh`, compare its date against the last actual change:
  - Gap analysis: `git log -1 --format=%cs -- 1-spec/` and `git status --porcelain -- 1-spec/` (uncommitted changes count as changes).
  - Completeness assessment: same over `2-design/`, `decisions/`, and `1-spec/`.
  - Changes dated after the recorded date while the line claims `fresh` are a finding. Changes on the same date are inconclusive from dates alone — report them as a note to review manually, not as a violation.

#### 7. Implementation log presence

- Every task with status `Done` or `In Progress` has a log at `3-code/implementation-log/<TASK-ID>.md`.

### Severity Classification

| Severity | Criteria | Examples |
|----------|----------|----------|
| **Critical** | An invariant is broken in a way that misleads phase gates or future agents | Assessment line claims `fresh` but artifacts changed later; index row pointing to a missing file; illegal status value; missed or wrong status propagation; duplicate IDs |
| **Important** | Weakens traceability or auditability without immediately misleading | Orphaned artifacts; index metadata not matching the file; missing implementation log for a `Done` or `In Progress` task; Blocked task without a noted reason; unresolvable Related Artifacts link |
| **Minor** | Cosmetic or inconclusive | Status-line formatting deviations; same-date freshness that cannot be verified from git; naming-style drift |

### Report Structure

1. **Summary line** — finding counts by severity, checks run, checks skipped (with the reason).
2. **Findings grouped by severity** (Critical first). For each: the violated invariant, the exact location (file, row, or line), and the suggested correction — naming the skill that owns the fix (e.g., re-run the gap analysis via `/SDLC-elicit`, apply the propagation step of `/SDLC-execute-task`, update the index row) or recommending a manual edit when no skill applies.
3. If everything passes: state it explicitly and list the checks performed.

### Rules

- **Read-only operation** — never modify any file. Do not flip staleness markers, fix index rows, or correct links yourself; report and recommend.
- **Git is read-only too** — use only inspecting commands (`git log`, `git status`, `git diff --name-only`).
- **Skip gracefully** — artifacts a project has not reached yet are skips, not findings.
- **Authorities over intuition** — validate against what `CLAUDE.md`, the phase files, and the templates actually specify; if a rule is ambiguous, report the ambiguity instead of enforcing an interpretation.
