---
name: SDLC-elicit
description: Interactive requirements elicitation for the Objectives phase. Use when defining, reviewing, or modifying stakeholders, goals, user stories, requirements, assumptions, or constraints. Also performs gap analysis on existing artifacts.
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

1. Read `1-objectives/CLAUDE.objectives.md` (phase instructions and existing artifact indexes).
2. Read `1-objectives/stakeholders.md` to understand existing stakeholders.

### Artifact Traceability Chain

Objectives artifacts form a traceability chain. Each level should decompose into the next:

1. **Stakeholder → Goals** — every stakeholder should have at least one associated goal. A stakeholder with no goals has no defined value proposition in the project.
2. **Goal → User Stories** — every goal should have at least one associated user story. Review the linked user stories against the goal's success criteria and flag coverage gaps.
3. **User Story → Requirements** — every user story should have at least one associated requirement. Review the linked requirements against the story's acceptance criteria and flag obvious coverage gaps.

These relationships are **traceability links with a coverage review heuristic**: check that linked artifacts appear to address the parent's success/acceptance criteria, and surface gaps for the user to evaluate. Completeness is a judgment call that requires domain expertise — the agent flags potential gaps, the human decides whether coverage is adequate.

### Elicitation Order (New Artifacts)

Elicitation is iterative — artifacts of different types often emerge together during a conversation (e.g., an assumption surfaces while discussing a user story, a constraint becomes apparent while defining a goal). Follow the user's natural train of thought rather than forcing a strict sequence.

That said, there is a **recommended starting sequence** for greenfield projects, because later artifacts depend on earlier ones:

1. **Stakeholders** — ask the user who uses, funds, operates, or is affected by the system. Update `1-objectives/stakeholders.md` with new entries (`STK-kebab-name`).
2. **Goals** — decompose vague ideas into concrete, measurable outcomes. Create `GOAL-<kebab-name>.md` from `1-objectives/goals/_template.md`.
3. **User Stories** — capabilities from the stakeholder perspective. Create `US-<kebab-name>.md` from `1-objectives/user-stories/_template.md`.
4. **Requirements** — formal, testable, derived from user stories or goals. Create `REQ-<CLASS>-<kebab-name>.md` from `1-objectives/requirements/_template.md`.

**Assumptions** and **Constraints** have no fixed position — capture them whenever they surface during discussion. Create `ASM-<kebab-name>.md` from `1-objectives/assumptions/_template.md` and `CON-<kebab-name>.md` from `1-objectives/constraints/_template.md`.

The only hard prerequisite: **at least one stakeholder must exist before creating goals, user stories, or requirements**, since these artifacts require a source stakeholder link.

You do not have to cover all artifact types in a single session. Work through whatever the user wants to discuss.

### Proactive Suggestions

The agent should actively suggest artifacts based on context:

- **After identifying a stakeholder**: suggest goals that align with their interests.
- **After creating a goal**: suggest assumptions that underpin it, constraints that limit it, and user stories that would realize it.
- **After creating a user story**: suggest requirements that formalize the acceptance criteria, and assumptions about user behavior or technical feasibility.
- **After creating an assumption**: suggest a verification plan and flag any existing artifacts that depend on it.
- **After creating a requirement**: suggest related requirements (e.g., a performance requirement to complement a functional one), and constraints that could affect implementation.
- **When reviewing existing artifacts**: suggest gaps — missing traceability links, uncovered stakeholder needs, requirements without corresponding user stories, etc.

Present suggestions as options the user can accept, modify, or decline. Do not create suggested artifacts without explicit user confirmation.

### Requirement Quality Check

When drafting a requirement to propose to the user, check it against these criteria before presenting it:

- **Verifiable** — every acceptance criterion must be measurable or testable. Flag vague language (e.g., "fast", "user-friendly", "secure") and suggest a concrete threshold or test condition.
- **Unambiguous** — there should be only one reasonable interpretation. Flag terms that different stakeholders could read differently (e.g., "large file", "real-time", "most users").
- **Consistent** — the requirement must not conflict with any existing requirement or constraint. Check against the current artifact set and flag contradictions.
- **Feasible** — if known constraints make the requirement unrealistic, flag the tension (e.g., a latency target that conflicts with a mandated third-party API).
- **Traceable** — the requirement must link to a source user story or goal. This is already enforced by the template, but verify the link exists and is valid.

Do not announce a "validation step" — apply these criteria silently while drafting. If any criterion fails, include an inline note in the proposal (e.g., _"Note: 'handle large files' is ambiguous — consider specifying a size threshold."_). The user can refine, accept as-is, or dismiss the note.

### Modifying Existing Artifacts

This skill can be used to review and modify any artifact in `1-objectives/`:

1. **Read the artifact** before proposing any changes.
2. **Present the proposed changes** clearly (what will change and why).
3. **Check downstream effects** — identify any artifacts in later phases (design decisions, tasks, etc.) that reference or depend on this artifact, and report them to the user before proceeding.
4. **Wait for user confirmation** before applying changes.

#### Status Downgrade on Modification

**When an artifact with `Status: Approved` is modified, its status must revert to `Draft`.**

- **Always warn the user before applying the change.** Clearly state: _"This artifact is currently Approved. Modifying it will reset its status to Draft, requiring re-approval."_
- Wait for the user to confirm they want to proceed.
- If the user confirms, apply the content changes **and** set `Status: Draft` in the same operation.
- If the modification also affects downstream artifacts (e.g., a requirement change that impacts design or tasks), flag those dependencies to the user.

**Exception**: metadata-only changes that do not alter the artifact's substance (e.g., fixing a typo in a link, correcting formatting) do not trigger a status downgrade. When in doubt, ask the user.

### Gap Analysis

This skill can analyze the current set of Objectives artifacts to identify gaps and inconsistencies. When the user asks for a gap analysis (or when reviewing artifacts suggests one):

#### 1. Goal Coverage Assessment

Read the `## Project Overview` section in `CLAUDE.md` and compare the stated project objective against the current set of approved and draft goals. Determine whether:

- The goals **collectively cover** the project objective — every major capability or outcome described in the overview is addressed by at least one goal.
- There are **aspects of the project objective not covered** by any goal — flag these as missing goals.
- Any goals **diverge from or exceed** the project objective — flag these as potentially out of scope.

Summarize the assessment before the detailed gap list: state whether the current goals are sufficient to realize the project objective, and list any uncovered areas.

#### 2. Gap Checks

Check for:

- **Missing traceability** — goals without user stories, user stories without requirements, requirements without a source goal or story.
- **Potential coverage gaps** — a goal whose linked user stories do not appear to address all its success criteria, or a user story whose linked requirements do not appear to cover all its acceptance criteria. Flag these as potential gaps for the user to evaluate.
- **Uncovered stakeholders** — stakeholders with no goals addressing their needs.
- **Orphaned artifacts** — requirements or assumptions that reference deleted or renamed artifacts.
- **Missing non-functional coverage** — functional requirements without corresponding performance, security, or usability requirements where appropriate.
- **Unverified assumptions** — assumptions with no verification plan.
- **Constraint coverage** — constraints that are not reflected in any requirement or design decision.

#### 3. Severity Classification

Classify each finding into one of three severity levels:

| Severity | Criteria | Examples |
|----------|----------|----------|
| **Critical** | Blocks phase gate advancement or leaves a core project objective unaddressed. Must be resolved before moving to Design. | Missing goal for a key part of the project objective; approved requirement referencing a deleted artifact; stakeholder with no goals at all; goal whose user stories appear insufficient to realize it |
| **Important** | Weakens traceability or coverage but does not block progress. Should be resolved before moving to Design. | User story without requirements; functional requirement missing non-functional counterpart; assumption without verification plan |
| **Minor** | Cosmetic or low-impact gaps that can be addressed later without risk. | Constraint not yet linked to a requirement; minor traceability link missing between related but non-dependent artifacts |

Present the findings grouped by severity level (Critical first, then Important, then Minor). Within each severity group, list findings by gap type. For each gap, suggest a concrete action (e.g., "create a user story for stakeholder X", "add a performance requirement for REQ-F-search"). Let the user decide which gaps to address and in what order.

### Interaction Style

- Ask one question at a time. Wait for the user's answer before proceeding.
- After gathering enough information, propose the content and ask for confirmation before creating the file. **Multiple artifacts of the same category may be proposed together** in a single batch (e.g., three goals at once, two assumptions at once). Present each artifact clearly so the user can approve, modify, or decline them individually.
- When the user confirms, create the file(s) from the appropriate template.
- **After completing an approved action or after the user declines a proposal, briefly summarize what was done, then ask the user how they want to proceed.** Do not jump directly into suggesting new artifacts.
- When the user asks for suggestions or is unsure what to do next, use the Proactive Suggestions guidelines above to offer relevant options.
- Surface potential conflicts between requirements immediately — follow the conflict resolution procedure in `CLAUDE.objectives.md`.

### Current State Tracking

Whenever the skill applies user-approved changes (creating, modifying, or deleting artifacts), update the `### Current State` subsection under `## Project Overview` in `CLAUDE.md` to reflect:

1. **Artifact types being elicited** — list which artifact types have been worked on in the Objectives phase (e.g., "Stakeholders defined; Goals and Assumptions drafted"). Update this incrementally as new types are touched.
2. **Last gap analysis** — if a gap analysis has been performed, record the date and a one-line result summary (e.g., "Gap analysis (2026-03-08): 2 Critical, 1 Important, 0 Minor"). Update this only when a gap analysis is actually run, not on every artifact change.

Gap analysis can be run at any time during elicitation — it is useful as a mid-session checkpoint to identify blind spots early. However, a gap analysis is **required before phase gate transition** (Objectives → Design). If artifacts have been created or modified since the last recorded gap analysis, append "(stale — artifacts changed since)" to the gap analysis line in Current State and remind the user that a fresh analysis is needed before advancing.

### Rules

- **Current State synchronization**: whenever artifacts are created, modified, or deleted, update `### Current State` in `CLAUDE.md` as described in the Current State Tracking section above. This update must happen in the same operation as the artifact change.
- **Index synchronization**: whenever an artifact file is created, modified, or deleted, update the corresponding index table in `1-objectives/CLAUDE.objectives.md` in the same operation. For new artifacts, add a row with all metadata columns (file link, status, priority, summary, etc.); for modifications, update the row to reflect the current metadata; for deletions, remove the row.
- All new artifacts start with `Status: Draft`. Never auto-approve — only a human can move to `Approved`.
- Modified `Approved` artifacts revert to `Draft` (see Status Downgrade above).
- Choose a short descriptive kebab-case name for each new artifact (check existing files to avoid duplicates). The name **is** the ID — there are no numeric sequences.
- Every artifact must have proper traceability links (Source stakeholder, Related goal, etc.).
- Follow the requirement class conventions: `REQ-F` (Functional), `REQ-PERF` (Performance), `REQ-SEC` (Security), etc.
- Use kebab-case for file names: `GOAL-user-authentication.md`, `REQ-F-login-with-email.md`.
- When creating files, fill in all template fields — do not leave placeholder brackets.