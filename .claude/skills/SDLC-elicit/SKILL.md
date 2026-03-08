---
name: SDLC-elicit
description: Interactive requirements elicitation for the Objectives phase. Use when defining, reviewing, or modifying stakeholders, goals, user stories, requirements, assumptions, or constraints.
---

## Instructions

You are running an interactive elicitation session for the Objectives phase of an AI-first SDLC project. This skill supports both **creating new artifacts** and **modifying existing ones**.

### Phase Validation

Before doing anything else, read the `### Current State` subsection under `## Project Overview` in `CLAUDE.md` and determine whether this skill should proceed:

1. **Project not initialized** — if the Current State indicates the project has not been initialized (e.g., mentions "not yet been initialized", "base scaffold", or lacks a real project description), **stop and inform the user**:
   - _"The project has not been initialized yet. Please run `/SDLC-init` first to set up the project description before starting elicitation."_
   - Do not proceed with elicitation. End the skill here.

2. **Project is in the Objectives phase** — if the Current State indicates the project is in the Objectives phase (e.g., mentions "Objectives phase", "elicitation", "defining goals/requirements", or no phase beyond Objectives has been started), **proceed normally** with the Setup steps below.

3. **Project has advanced beyond Objectives** — if the Current State indicates the project is in Design, Code, or Deploy phase, **warn the user**:
   - _"The project is currently in the [detected phase] phase. Modifying Objectives artifacts at this stage may impact downstream design, tasks, or decisions."_
   - If the user wants to **review or modify existing artifacts**, proceed but flag any downstream dependencies that could be affected.
   - If the user wants to **create new artifacts**, proceed but remind them to check whether downstream phases need to be updated afterward.

### Setup

1. Read `1-objectives/CLAUDE.objectives.md` (phase instructions).
2. Read `1-objectives/stakeholders.md` to understand existing stakeholders.
3. Scan the indexes in `CLAUDE.objectives.md` (Goals, User Stories, Requirements, Assumptions, Constraints) to understand what already exists.
4. If the user asks to modify a specific artifact, read its file before proposing changes.

### Elicitation Order (New Artifacts)

Follow the prescribed order from `CLAUDE.objectives.md`:

1. **Stakeholders** — ask the user who uses, funds, operates, or is affected by the system. Update `1-objectives/stakeholders.md` with new entries (`STK-kebab-name`).
2. **Goals** — decompose vague ideas into concrete, measurable outcomes. Create `GOAL-<kebab-name>.md` from `1-objectives/goals/_template.md`.
3. **Assumptions** — surface beliefs early. Create `ASM-<kebab-name>.md` from `1-objectives/assumptions/_template.md`.
4. **Constraints** — document hard limits. Create `CON-<kebab-name>.md` from `1-objectives/constraints/_template.md`.
5. **User Stories** — capabilities from the stakeholder perspective. Create `US-<kebab-name>.md` from `1-objectives/user-stories/_template.md`.
6. **Requirements** — formal, testable, derived from user stories or goals. Create `REQ-<CLASS>-<kebab-name>.md` from `1-objectives/requirements/_template.md`.

You do not have to cover all artifact types in a single session. Work through whatever the user wants to discuss. If starting from scratch, begin with stakeholders.

### Proactive Suggestions

The agent should actively suggest artifacts based on context:

- **After creating a goal**: suggest assumptions that underpin it, constraints that limit it, and user stories that would realize it.
- **After creating a user story**: suggest requirements that formalize the acceptance criteria, and assumptions about user behavior or technical feasibility.
- **After creating a requirement**: suggest related requirements (e.g., a performance requirement to complement a functional one), and constraints that could affect implementation.
- **After creating an assumption**: suggest a verification plan and flag any existing artifacts that depend on it.
- **After identifying a stakeholder**: suggest goals that align with their interests.
- **When reviewing existing artifacts**: suggest gaps — missing traceability links, uncovered stakeholder needs, requirements without corresponding user stories, etc.

Present suggestions as options the user can accept, modify, or decline. Do not create suggested artifacts without explicit user confirmation.

### Modifying Existing Artifacts

This skill can be used to review and modify any artifact in `1-objectives/`:

1. **Read the artifact** before proposing any changes.
2. **Present the proposed changes** clearly (what will change and why).
3. **Wait for user confirmation** before applying changes.
4. **Update the index row** in `CLAUDE.objectives.md` to match the modified artifact.

#### Status Downgrade on Modification

**When an artifact with `Status: Approved` is modified, its status must revert to `Draft`.**

- **Always warn the user before applying the change.** Clearly state: _"This artifact is currently Approved. Modifying it will reset its status to Draft, requiring re-approval."_
- Wait for the user to confirm they want to proceed.
- If the user confirms, apply the content changes **and** set `Status: Draft` in the same operation.
- Update the index row status accordingly.
- If the modification also affects downstream artifacts (e.g., a requirement change that impacts design or tasks), flag those dependencies to the user.

**Exception**: metadata-only changes that do not alter the artifact's substance (e.g., fixing a typo in a link, correcting formatting) do not trigger a status downgrade. When in doubt, ask the user.

### Interaction Style

- Ask one question at a time. Wait for the user's answer before proceeding.
- After gathering enough information for an artifact, propose the content and ask for confirmation before creating the file.
- When the user confirms, create the file from the appropriate template and update the corresponding index table in `CLAUDE.objectives.md`.
- Proactively suggest related artifacts (see Proactive Suggestions above).
- Surface potential conflicts between requirements immediately — follow the conflict resolution procedure in `CLAUDE.objectives.md`.

### Rules

- All new artifacts start with `Status: Draft`. Never auto-approve — only a human can move to `Approved`.
- Modified `Approved` artifacts revert to `Draft` (see Status Downgrade above).
- Choose a short descriptive kebab-case name for each new artifact (check existing files to avoid duplicates). The name **is** the ID — there are no numeric sequences.
- Every artifact must have proper traceability links (Source stakeholder, Related goal, etc.).
- Follow the requirement class conventions: `REQ-F` (Functional), `REQ-PERF` (Performance), `REQ-SEC` (Security), etc.
- Use kebab-case for file names: `GOAL-user-authentication.md`, `REQ-F-login-with-email.md`.
- When creating files, fill in all template fields — do not leave placeholder brackets.

### Session End

When the user wants to stop, summarize what was created or modified in this session and suggest what to elicit next.
