---
name: SDLC-design
description: Draft or update design documents based on approved specification. Use when translating requirements into technical specifications. Covers architecture, data model, API design, and design completeness assessment.
---

## Instructions

You are working in the Design phase, creating or updating architecture, data model, or API design documents.

### Phase Validation

Before doing anything else, read the `### Current State` subsection under `## Project Overview` in `CLAUDE.md` and determine which phase the project is in. Then follow the matching case below:

1. **Project not initialized** — the Current State lacks a real project description (e.g., mentions "not yet been initialized" or "base scaffold"). **Stop**, recommend `/SDLC-init`, and do not proceed.

2. **Project is in the Specification phase** — the Current State mentions "Specification phase", lists specification artifacts being drafted, or no phase beyond Specification has been started.

   Read the index tables in `1-spec/CLAUDE.spec.md` and evaluate the Spec → Design phase gate preconditions:

   - **(a)** At least one goal with `Status: Approved`
   - **(b)** At least one requirement with `Status: Approved`
   - **(c)** Gap analysis recorded in the Current State, fresh (not stale), and with no Critical gaps

   Then respond based on the results:

   - **If (a) or (b) are not met** — elicitation is incomplete. **Stop**, list what is missing, recommend `/SDLC-elicit`, and do not proceed.
   - **If (a) and (b) are met but (c) is not met** (no gap analysis, stale, or Critical gaps) — **strongly recommend** running a gap analysis via `/SDLC-elicit` before design. Proceed only on explicit user confirmation.
   - **If all preconditions are met** — inform the user of any remaining Important/Minor gaps (non-blocking) and proceed with Setup.

3. **Project is in the Design phase** — the Current State indicates the project is in the Design phase (e.g., mentions "Design phase", "architecture", "design documents being worked on", or no phase beyond Design has been started), **proceed normally** with the Setup steps below.

4. **Project has advanced beyond Design** — the Current State indicates the project is in Code or Deploy phase. **Warn** that modifying Design artifacts may impact downstream tasks or deployed code. If the user confirms, proceed but flag downstream dependencies that could be affected.

### Setup

1. Read `2-design/CLAUDE.design.md` (phase instructions and decisions index).
2. Read any decisions whose trigger conditions apply.

### Design Principle

The goal of this phase is to produce the **simplest design that makes every approved requirement satisfiable**. Complexity is only justified when a simpler alternative fails to satisfy a requirement or violates a constraint. When in doubt, prefer fewer components, fewer abstractions, and more conventional technology choices. When requirements create design tension, prioritize Must-have requirements over Should-have — a design that simply satisfies all Must-haves and most Should-haves is preferable to a complex design that satisfies everything.

### Workflow

#### 1. Assess Current State

Before doing anything else, determine what already exists:

- Read `2-design/architecture.md`, `2-design/data-model.md`, and `2-design/api-design.md` to understand what design work has been done.
- Summarize the current design state to the user: which documents exist, which are empty or incomplete, and what the logical next step is.

#### 2. Load Specification Artifacts

Read **only the index tables** in `1-spec/CLAUDE.spec.md` — do **not** open individual artifact files yet. Use the summary column in each index to determine relevance:

- **Requirements Index** — identify which requirements apply to the current design work.
- **Goals Index** — identify which goals the design must satisfy.
- **Constraints Index** — identify hard limits that affect design choices.
- **Assumptions Index** — identify assumptions that inform feasibility and trade-offs.
- **User Stories Index** — identify stories that provide user-facing context.

**Then**, for each artifact whose summary indicates it is relevant to the design task at hand, read the individual file to get full details. Skip artifacts that are clearly unrelated — there is no need to read every file.

**Flag draft artifacts**: if any requirements or user stories relevant to the current design work are still in `Draft` status, warn the user before proceeding. Draft artifacts may change, and designing against them introduces risk.

#### 3. Build the Simplest Satisfying Design

Follow this progression — each step builds on the previous one. At every step, apply the Design Principle: include only what is needed to satisfy approved requirements and respect constraints.

**a. Architecture** (`2-design/architecture.md`)
- Define the system's components and their responsibilities.
- Document component interactions using Mermaid diagrams.
- Reference requirements by ID (e.g., `REQ-F-search-by-name`) for traceability.

**b. Data Model** (`2-design/data-model.md`)
- Define the minimal data structures needed to support the architecture. Avoid entities or relationships not required by any approved requirement.
- Document types, relationships, and lifecycle (creation, updates, deletion).
- Align with component responsibilities established in the architecture.

**c. Interface Design** (`2-design/api-design.md`)
- Define only the endpoints and contracts needed by the components and user stories. Avoid speculative APIs for features not backed by approved requirements.
- Follow REST conventions or document deviations.
- Define request/response formats, error handling, and status codes.
- Consider versioning strategy if the API may evolve.

The user does not have to complete all three documents in a single session. If the architecture is not yet defined, start there. If the architecture exists but the data model is missing, start with the data model, and so on.

#### 4. Verify Requirement and Constraint Coverage

Before presenting the draft, cross-reference the design against approved requirements **and** constraints:

- For each approved requirement, confirm it is satisfiable by the proposed design.
- For each constraint in `1-spec/constraints/`, confirm the design does not violate it. If a design choice is in tension with a constraint, surface the conflict and explain the trade-off.
- For each assumption the design depends on, check its verification status. If unverified, note it as a design risk.
- List any requirement that is not yet covered and explain why (e.g., depends on a document not yet drafted, requires a decision from the user).
- If a Must-have requirement is not covered or a constraint is violated, the design is incomplete — address it before proceeding, or explicitly flag it as a gap that needs resolution.

This verification applies to whatever document(s) are being drafted in the current session, not necessarily all three at once.

#### 5. Present and Write

- **Present the draft** to the user for review before writing. Highlight:
  - The coverage verification results (which requirements are addressed, which are not)
  - Design trade-offs made
- **After user approval**, write the document.

### Modifying Existing Design Documents

This skill can be used to revise design documents or propagate decision changes. When modifying existing content:

1. **Read the document** before proposing any changes.
2. **Check downstream effects** — identify tasks in `3-code/tasks.md` and decisions in `decisions/` that reference or depend on the content being changed. Report these to the user before proceeding.
3. **Present the proposed changes** clearly (what will change, why, and which downstream artifacts are affected).
4. **Wait for user confirmation** before applying changes.

### Design Completeness Assessment

This skill can assess the overall readiness of the design phase. When the user requests a completeness assessment, or before Design → Code transition:

#### 1. Document Completeness

Check which design documents have content and which are empty or incomplete. For documents with content, assess whether they cover the scope implied by the architecture (e.g., if the architecture defines three components, does the API design define endpoints for all three?).

#### 2. Requirement Coverage

Cross-reference **all** approved requirements against the full set of design documents — not just the current session's work. For each uncovered requirement, note which design document should address it.

#### 3. Constraint Compliance

Verify no design choice across any document violates a constraint.

#### 4. Assumption Risk

Identify assumptions the design depends on that are still unverified. A design built on an unverified assumption may need revision if the assumption proves false.

#### 5. Decision Coverage

Review design documents for significant technical choices that are not captured as `DEC-*` artifacts. Implicit decisions embedded in documents without a corresponding record weaken traceability and risk inconsistency.

#### 6. Severity Classification

| Severity | Criteria | Examples |
|----------|----------|----------|
| **Critical** | Blocks Design → Code transition or leaves a Must-have requirement unaddressed | Empty architecture document; Must-have requirement not covered by any design document; constraint violated by a design choice |
| **Important** | Weakens design quality or traceability but does not block progress | Implicit decision not recorded as `DEC-*`; Should-have requirement not covered; design depends on unverified high risk assumption; design document incomplete for its scope |
| **Minor** | Low-impact gaps that can be addressed during implementation | Missing Mermaid diagram; design depends on unverified low risk assumption; minor traceability link missing |

Present findings grouped by severity (Critical first). For each finding, suggest a concrete action. Let the user decide which to address and in what order.

#### 7. Next Step Guidance

After the completeness assessment is finalized and there are **no Critical findings**, inform the user that the next step is **component identification** via `/SDLC-decompose`. This skill analyzes the design artifacts to identify distinct software components and creates per-component directories.

For complex projects, suggest running the assessment iteratively — addressing findings, updating the design, and re-assessing — until results converge (i.e., no new Critical or Important findings emerge between successive runs).

### Decision Triggers

Decisions must be captured whenever the user shapes the design — not only when a technical pattern emerges. Follow the recording, deprecation, and supersession procedures in [`decisions/PROCEDURES.md`](../../../decisions/PROCEDURES.md).

**When to create a new decision:**
- A significant technical pattern emerges (error handling, data flow, security, naming conventions, etc.).
- The user **approves a design choice** — the approval itself is the decision; record it immediately.
- The user **expresses a preference** (e.g., "I'd rather use SQLite than PostgreSQL", "let's keep the API REST-only") — treat every stated preference as a decision to record.

**When to modify an existing decision:**
- The user **changes their mind** about a previously recorded decision — follow the deprecation or supersession procedure in `PROCEDURES.md`.
- New information invalidates or narrows a prior decision — propose an update and wait for approval before modifying.

**Rules:**
- Do **not** silently embed decisions into design documents without recording them as `DEC-*` artifacts.
- Do **not** wait until the end of the session — record or update decisions as soon as they are confirmed.
- When in doubt about whether something qualifies as a decision, surface it to the user and ask.

### Proactive Suggestions

After completing a design action, suggest relevant follow-up work:

- **After drafting architecture**: suggest data model implications (entities and relationships implied by the components), potential decisions to record (technology choices, component boundaries), and constraints to verify against the proposed structure.
- **After drafting data model**: suggest API implications (endpoints the data structures imply), and flag any architecture assumptions that the data model reveals.
- **After drafting API design**: suggest integration or security decisions that may need recording, and cross-check against user stories for completeness.
- **After modifying a design document**: flag decisions or other design documents that may be affected by the change.

Present suggestions as options the user can accept, modify, or decline. Do not act on suggestions without explicit user confirmation.

### Interaction Style

- Ask one topic at a time — closely related design questions may be grouped together (e.g., "Which components need persistent storage, and what are their data access patterns?"), but avoid mixing unrelated topics in a single turn. Wait for the user's answer before moving to the next topic.
- When the architecture involves trade-offs or multiple valid approaches, present the options with pros and cons and let the user choose.
- After gathering enough information, propose the content and ask for confirmation before writing the file. **Multiple related design sections may be proposed together** in a single batch (e.g., two component descriptions at once, several API endpoints at once). Present each section clearly so the user can approve, modify, or decline them individually.
- When the user confirms, write the document or section.
- **After completing an approved action or after the user declines a proposal, briefly summarize what was done, then ask the user how they want to proceed.** Do not jump directly into the next design step.
- When the user asks for suggestions or is unsure what to do next, use the Proactive Suggestions guidelines above and suggest the next design step based on the progression (architecture → data model → interfaces) and the current state of the design documents.
- Surface potential conflicts between requirements and design constraints immediately — do not resolve silently. When a design choice impacts multiple documents, note the cross-document implications.

### Current State Tracking

Whenever the skill applies user-approved changes (creating or updating design documents, recording decisions), update the `### Current State` subsection under `## Project Overview` in `CLAUDE.md` to reflect:

1. **Phase transition** — if this is the first design work in the project (transitioning from Specification to Design), **rewrite** the Current State section: replace the Specification-phase information (artifact counts, statuses, gap analysis details, etc.) with a fresh Design-phase summary.
2. **Design documents status** — list which design documents have content and which are still empty or incomplete (e.g., "Architecture drafted; data model and API design pending"). Update this incrementally as documents are created or modified.
3. **Decisions recorded** — note the count of decisions created (e.g., "3 decisions recorded"). Update this when new decisions are recorded.
4. **Last completeness assessment** — if a completeness assessment has been performed, record the date and a one-line result summary (e.g., "Completeness assessment (2026-03-10): 0 Critical, 1 Important, 2 Minor"). Update this only when an assessment is actually run, not on every design change. If design documents or decisions have been modified since the last recorded assessment, append "(stale — design changed since)" and remind the user that a fresh assessment is needed before advancing to Code.

### Rules

- **Current State synchronization**: whenever design documents are created, updated, or decisions are recorded, update `### Current State` in `CLAUDE.md` as described in the Current State Tracking section above. This update must happen in the same operation as the design change.
- Always reference requirements by ID (e.g., `REQ-F-search-by-name`) for traceability.
- Use Mermaid diagrams where they add clarity.
- If a requirement is ambiguous or contradictory, surface it — do not resolve silently.
