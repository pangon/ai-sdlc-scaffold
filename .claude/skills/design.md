# Skill: /design

Draft or update design documents based on approved objectives.

## Instructions

You are working in the Design phase, creating or updating architecture, data model, or API design documents.

### Setup

1. Read `CLAUDE.md` (root instructions) and `2-design/CLAUDE.design.md` (phase instructions). Follow both throughout this session.
2. Read the Decisions index in `CLAUDE.design.md` and load any decisions whose trigger conditions apply.
3. Read the indexes in `1-objectives/CLAUDE.objectives.md` to understand approved requirements, goals, constraints, and assumptions.

### Workflow

1. **Ask the user** which design document to work on:
   - `2-design/architecture.md` — system architecture overview
   - `2-design/data-model.md` — data structures and schemas
   - `2-design/api-design.md` — API specifications
   - Or a new design document if needed

2. **Read approved artifacts**: before drafting, read all `Approved` requirements, relevant goals, constraints, and assumptions. Reference them by ID in the design.

3. **Read the current state** of the target design document.

4. **Draft or update** the document following the guidelines in `CLAUDE.design.md`:
   - Architecture: simplest design that satisfies requirements, component responsibilities, Mermaid diagrams
   - Data model: clear schemas with types, relationships, lifecycle
   - API design: REST conventions, request/response formats, error handling, versioning

5. **Present the draft** to the user for review before writing. Highlight:
   - Which requirements are addressed
   - Any requirements that cannot be addressed yet (and why)
   - Design trade-offs made

6. **After user approval**, write the document.

### Decision Triggers

While designing, if a significant decision emerges (error handling patterns, data flow conventions, security patterns, etc.):
- Surface it to the user
- Suggest recording it as a decision using the `/decide` skill or the procedure in `CLAUDE.md`
- Do not silently embed decisions into design documents without recording them

### Rules

- Always reference requirements by ID (e.g., `REQ-F001`) for traceability.
- Follow the guidelines in `CLAUDE.design.md` for each document type.
- Use Mermaid diagrams where they add clarity.
- Default to the simplest design that satisfies all requirements.
- If a requirement is ambiguous or contradictory, surface it — do not resolve silently.
- Check that constraints from `1-objectives/constraints/` are respected in the design.
