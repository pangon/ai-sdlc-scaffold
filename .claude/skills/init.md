# Skill: /init

Guided project initialization — walks through all customization points in the scaffold.

## Instructions

You are guiding the user through the initial setup of a new project based on the AI SDLC Scaffold. This replaces manual search for `<!-- CUSTOMIZE -->` markers and placeholder content.

### Setup

1. Read `CLAUDE.md` (root instructions).
2. Read `README.md` for the Customization Checklist.

### Workflow

Walk the user through each customization point **interactively**, one at a time. For each point, explain what it is, why it matters, and ask the user to provide the information. Then apply the change.

#### Step 1: Project Overview in `CLAUDE.md`

1. Read `CLAUDE.md` and locate the `## Project Overview` section (contains `<!-- CUSTOMIZE: Replace this section with a description of your project. -->`).
2. Ask the user to describe their project: what it does, what problem it solves, what technology stack they plan to use.
3. Replace the placeholder comment and any generic text with a concise project description written for AI agent consumption (clear, factual, structured).

#### Step 2: Stakeholders in `1-objectives/stakeholders.md`

1. Read `1-objectives/stakeholders.md`.
2. Ask the user: who uses, funds, operates, or is affected by the system?
3. For each stakeholder, collect: role name, description, interests, and influence level (High/Medium/Low).
4. Replace the placeholder row with real stakeholder entries. Assign sequential `STK-NNN` IDs.

#### Step 3: Component Guidelines in `3-code/CLAUDE.code.md`

1. Read `3-code/CLAUDE.code.md` and locate the `## Component Guidelines` section (contains `<!-- CUSTOMIZE: Add an entry for each component/codebase. -->`).
2. Ask the user about their codebase(s): what components they plan to have, what runtime/language/framework each uses, what their responsibilities are.
3. Add a component entry for each one, following the template block already present in the file.
4. If the user is unsure, it is acceptable to skip this step and come back later — note that it's incomplete.

#### Step 4: Environment Configuration in `4-deploy/CLAUDE.deploy.md`

1. Read `4-deploy/CLAUDE.deploy.md` and locate the `## Environment Configuration` section (contains `<!-- CUSTOMIZE: Fill in environment-specific details. -->`).
2. Ask the user about their target environments (development, staging, production, or others).
3. Fill in the environment table with purpose and relevant notes.
4. If the user is unsure, keep the defaults and remove the CUSTOMIZE comment — they can update later.

#### Step 5: Review DEC-001 Example Decision

1. Read `2-design/decisions/DEC-001-error-format.md`.
2. Explain to the user that this is an example decision showing the pattern. Ask whether they want to:
   - **Keep it** — if relevant to their project (e.g., they're building an API)
   - **Adapt it** — modify the error format for their specific needs
   - **Remove it** — if not applicable (clean slate)
3. Apply the user's choice. If removing, also delete `DEC-001-error-format.history.md` and remove the DEC-001 rows from the decision indexes in `CLAUDE.design.md` and `CLAUDE.code.md`.

#### Step 6: Review Phase Instruction Files

1. Ask the user about their tech stack if not already known from previous steps.
2. Read through each `CLAUDE.<phase>.md` file and flag any conventions or defaults that may not fit their tech stack (e.g., REST API conventions if they're building a CLI tool, or testing conventions that don't match their framework).
3. Propose targeted adjustments — do not rewrite the files, just tweak what doesn't fit.

#### Step 7: Summary

Present a summary of all changes made:
- Project description: set / skipped
- Stakeholders: N defined / skipped
- Components: N defined / skipped
- Environments: configured / defaults kept
- DEC-001: kept / adapted / removed
- Phase files: N adjustments made / no changes needed

List any items that were skipped so the user can come back to them later.

### Rules

- Work through steps **sequentially** — each step may inform the next.
- Ask the user for input at each step. Do not assume or invent project details.
- Write content optimized for AI agent consumption: clear, structured, factual.
- Remove `<!-- CUSTOMIZE -->` comments and placeholder text as each section is filled in.
- If the user wants to skip a step, that's fine — note it in the summary.
- Do not modify any file structure, templates, or procedures — only fill in project-specific content.
