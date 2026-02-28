# Skill: /phase-gate

Check phase gate preconditions for a specific phase transition.

## Instructions

You are checking whether the preconditions for advancing to the next phase are met.

### Setup

1. Read `CLAUDE.md` (root instructions) — especially the Phase Gates section.
2. Read the phase instruction files relevant to the transition being checked.

### Usage

The user may specify which transition to check (e.g., `/phase-gate design`). If not specified, ask which transition to evaluate:

- **design** — Objectives → Design
- **code** — Design → Code
- **deploy** — Code → Deploy

### Precondition Checks

#### Objectives → Design

Read `1-objectives/CLAUDE.objectives.md` indexes and `1-objectives/stakeholders.md`.

| Precondition | How to check |
|-------------|-------------|
| At least one goal Approved | Scan Goals index for any `Approved` status |
| At least one requirement Approved | Scan Requirements index for any `Approved` status |
| Stakeholders defined | Check `stakeholders.md` has at least one non-placeholder entry |

#### Design → Code

Read `2-design/architecture.md` and cross-reference `1-objectives/CLAUDE.objectives.md` requirements index with `3-code/tasks.md`.

| Precondition | How to check |
|-------------|-------------|
| `architecture.md` has content | File exists and has substantive content beyond the stub/template |
| At least one requirement has a corresponding task | Cross-reference `Approved` requirements with `Req` column in `tasks.md` |

#### Code → Deploy

Read `3-code/tasks.md`.

| Precondition | How to check |
|-------------|-------------|
| At least one task Done | Scan task table for any `Done` status |

### Report Format

For each precondition, report:
- **MET** or **NOT MET**
- Brief evidence (e.g., "GOAL-001 is Approved", "architecture.md is still a stub")

Then state the overall gate result:
- **GATE PASSED** — all preconditions met, safe to proceed
- **GATE NOT PASSED** — list what's missing, suggest specific actions

### Rules

- This is a read-only operation — do not modify any files.
- Gates are advisory (per `CLAUDE.md`): warn the user if not met, but proceed if they confirm.
- If the user wants to proceed despite unmet preconditions, acknowledge the override but note the risks.
