---
name: SDLC-design
description: Draft or update design documents based on approved objectives. Use when translating requirements into technical specifications. Covers architecture, data model, and API design.
---

## Instructions

You are working in the Design phase, creating or updating architecture, data model, or API design documents.

### Setup

1. Read `2-design/CLAUDE.design.md` (phase instructions).
2. Read the Decisions index in `CLAUDE.design.md` and load any decisions whose trigger conditions apply.

### Workflow

#### 1. Assess Current State

Before doing anything else, determine what already exists:

- Read `2-design/architecture.md`, `2-design/data-model.md`, and `2-design/api-design.md` to understand what design work has been done.
- Summarize the current design state to the user: which documents exist, which are empty or incomplete, and what the logical next step is.

#### 2. Load Objectives Artifacts

Read **only the index tables** in `1-objectives/CLAUDE.objectives.md` — do **not** open individual artifact files yet. Use the summary column in each index to determine relevance:

- **Requirements Index** — identify which requirements apply to the current design work.
- **Goals Index** — identify which goals the design must satisfy.
- **Constraints Index** — identify hard limits that affect design choices.
- **Assumptions Index** — identify assumptions that inform feasibility and trade-offs.
- **User Stories Index** — identify stories that provide user-facing context.

**Then**, for each artifact whose summary indicates it is relevant to the design task at hand, read the individual file to get full details. Skip artifacts that are clearly unrelated — there is no need to read every file.

**Flag draft artifacts**: if any requirements, user stories, or assumptions relevant to the current design work are still in `Draft` status, warn the user before proceeding. Draft artifacts may change, and designing against them introduces risk.

#### 3. Guide the Design Process

Follow this progression — each step builds on the previous one:

**a. Architecture** (`2-design/architecture.md`)
- Define the system's components and their responsibilities.
- Choose the simplest architecture that satisfies all approved requirements.
- Document component interactions using Mermaid diagrams.
- Reference requirements by ID (e.g., `REQ-F001`) for traceability.
- Ensure constraints from `1-objectives/constraints/` are respected.

**b. Data Model** (`2-design/data-model.md`)
- Define data structures and schemas informed by the architecture.
- Document types, relationships, and lifecycle (creation, updates, deletion).
- Align with component responsibilities established in the architecture.

**c. Interface Design** (`2-design/api-design.md`)
- Specify APIs and contracts between the components defined in the architecture.
- Follow REST conventions or document deviations.
- Define request/response formats, error handling, and status codes.
- Apply error format conventions from relevant decisions.

The user does not have to complete all three documents in a single session. If the architecture is not yet defined, start there. If the architecture exists but the data model is missing, start with the data model, and so on.

#### 4. Present and Write

- **Present the draft** to the user for review before writing. Highlight:
  - Which requirements are addressed
  - Any requirements that cannot be addressed yet (and why)
  - Design trade-offs made
- **After user approval**, write the document.

### Decision Triggers

While designing, if a significant decision emerges (error handling patterns, data flow conventions, security patterns, etc.):
- Surface it to the user
- Suggest recording it as a decision using the procedure in `CLAUDE.md`
- Do not silently embed decisions into design documents without recording them

### Interaction Style

- Ask one question at a time. Wait for the user's answer before proceeding.
- After gathering enough information for a design section or document, propose the content and ask for confirmation before writing the file.
- When the architecture involves trade-offs or multiple valid approaches, present the options with pros and cons and let the user choose.
- Proactively suggest the next design step based on the progression (architecture → data model → interfaces).
- Surface potential conflicts between requirements and design constraints immediately — do not resolve silently.
- When a design choice impacts multiple documents, note the cross-document implications.

### Rules

- Always reference requirements by ID (e.g., `REQ-F001`) for traceability.
- Follow the guidelines in `CLAUDE.design.md` for each document type.
- Use Mermaid diagrams where they add clarity.
- Default to the simplest design that satisfies all requirements.
- If a requirement is ambiguous or contradictory, surface it — do not resolve silently.
- Check that constraints from `1-objectives/constraints/` are respected in the design.

### Session End

When the user wants to stop, summarize what was created or modified in this session and suggest the next design step to tackle.
