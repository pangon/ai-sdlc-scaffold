# Skill: /elicit

Interactive requirements elicitation for the Objectives phase.

## Instructions

You are running an interactive elicitation session for the Objectives phase of an AI-first SDLC project.

### Setup

1. Read `CLAUDE.md` (root instructions) and `1-objectives/CLAUDE.objectives.md` (phase instructions). Follow both throughout this session.
2. Read `1-objectives/stakeholders.md` to understand existing stakeholders.
3. Scan the indexes in `CLAUDE.objectives.md` (Goals, User Stories, Requirements, Assumptions, Constraints) to understand what already exists.

### Elicitation Order

Follow the prescribed order from `CLAUDE.objectives.md`:

1. **Stakeholders** — ask the user who uses, funds, operates, or is affected by the system. Update `1-objectives/stakeholders.md` with new entries (STK-NNN).
2. **Goals** — decompose vague ideas into concrete, measurable outcomes. Create `GOAL-NNN-<name>.md` from `1-objectives/goals/_template.md`.
3. **Assumptions** — surface beliefs early. Create `ASM-NNN-<name>.md` from `1-objectives/assumptions/_template.md`.
4. **Constraints** — document hard limits. Create `CON-NNN-<name>.md` from `1-objectives/constraints/_template.md`.
5. **User Stories** — capabilities from the stakeholder perspective. Create `US-NNN-<name>.md` from `1-objectives/user-stories/_template.md`.
6. **Requirements** — formal, testable, derived from user stories or goals. Create `REQ-<PREFIX>NNN-<name>.md` from `1-objectives/requirements/_template.md`.

You do not have to cover all artifact types in a single session. Work through whatever the user wants to discuss. If starting from scratch, begin with stakeholders.

### Interaction Style

- Ask one question at a time. Wait for the user's answer before proceeding.
- After gathering enough information for an artifact, propose the content and ask for confirmation before creating the file.
- When the user confirms, create the file from the appropriate template and update the corresponding index table in `CLAUDE.objectives.md`.
- Proactively suggest related artifacts (e.g., after a goal, suggest assumptions or constraints that might apply).
- Surface potential conflicts between requirements immediately — follow the conflict resolution procedure in `CLAUDE.objectives.md`.

### Rules

- All new artifacts start with `Status: Draft`. Never auto-approve — only a human can move to `Approved`.
- Assign the next sequential ID for each artifact type (check existing files to determine the next number).
- Every artifact must have proper traceability links (Source stakeholder, Related goal, etc.).
- Follow the ID prefix conventions for requirements: `REQ-F` (Functional), `REQ-PERF` (Performance), `REQ-SEC` (Security), etc.
- Use kebab-case for file names: `GOAL-001-user-authentication.md`.
- When creating files, fill in all template fields — do not leave placeholder brackets.

### Session End

When the user wants to stop, summarize what was created in this session and suggest what to elicit next.
