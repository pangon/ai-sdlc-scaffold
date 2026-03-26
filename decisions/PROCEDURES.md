# Decision Procedures

Read this file only when you need to **record**, **deprecate**, or **supersede** a decision. For normal task execution, follow the navigation rules in [CLAUDE.md](../CLAUDE.md).

---

## Recording a New Decision

When a significant decision, pattern, or constraint emerges:

1. Choose a short descriptive kebab-case name that captures the decision (e.g., `use-postgres`, `error-response-format`).
2. Create `DEC-kebab-name.md` from [`_template.md`](_template.md) and fill in all fields.
3. Create `DEC-kebab-name.history.md` from [`_template.history.md`](_template.history.md) and fill in all fields.
4. Add an entry (with a File column linking to the new file) to the decisions index of **every** phase whose trigger conditions are met. The decision template defines trigger conditions per phase (Specification, Design, Code, Deploy). Check each phase and add the decision to all matching indexes:
   - `1-spec/CLAUDE.spec.md` — if the decision has a "Specification phase" trigger
   - `2-design/CLAUDE.design.md` — if the decision has a "Design phase" trigger
   - **Code phase** — if the decision has a "Code phase" trigger, add it to the `## Relevant Decisions` table of each affected component's `3-code/<component>/CLAUDE.component.md`. Determine which components are affected by the decision's **Scope** field and trigger description. If no per-component directories exist yet (i.e., components have not been identified), skip the Code phase index entirely — the decision will be picked up when components are created.
   - `4-deploy/CLAUDE.deploy.md` — if the decision has a "Deploy phase" trigger

   Do **not** limit the entry to only the current working phase.

### Human involvement vocabulary

Use these values in the `*.history.md` file:

| Value | Meaning |
|-------|---------|
| `human-decided` | Human made the decision; AI had no significant role |
| `ai-proposed/human-approved` | AI proposed; human explicitly approved |
| `ai-proposed/auto-accepted` | AI proposed and recorded without explicit human approval |

---

## Deprecating or Superseding a Decision

A decision should be deprecated when no longer relevant, or superseded when a new decision replaces it.

**Never deprecate or supersede silently.** Always surface the proposal to the user first.

1. **Identify the candidate**: note the decision ID (e.g., `DEC-use-postgres`) and reason for retirement.
2. **Read both files**: `DEC-kebab-name.md` and `DEC-kebab-name.history.md` to understand full context.
3. **Ask the user.** Present:
   - Why the decision is no longer valid or should be replaced
   - Whether existing code, infrastructure, or process still depends on it
   - The proposed action: deprecate (retire) or supersede (replace with new decision)
4. **Wait for explicit approval** before modifying any file.
5. **Apply:**

   **If deprecating:**
   - In the decision file: change `**Status**` to `Deprecated`.
   - In the history file: append a changelog entry with date, change, and involvement type.
   - Remove the decision from every phase index (`1-spec/CLAUDE.spec.md`, `2-design/CLAUDE.design.md`, affected `3-code/<component>/CLAUDE.component.md` files, `4-deploy/CLAUDE.deploy.md`).

   **If superseding:**
   - Create the replacement decision following the recording procedure above.
   - In the old decision file: change `**Status**` to `Superseded by DEC-new-name`.
   - In the old history file: append changelog entry.
   - In every phase index: replace the old row with the new one.

6. **Check design documents for impact**: search `2-design/architecture.md`, `2-design/data-model.md`, and `2-design/api-design.md` for references to the retired decision.
   - If references are found, list the affected sections and give a brief assessment of the likely impact.
   - If you are already operating within the `/SDLC-design` skill, proceed with proposing the design updates.
   - Otherwise, do **not** modify design documents here — design changes require the full context of the `/SDLC-design` skill. Recommend the user run `/SDLC-design` to apply the necessary updates.
7. **Verify**: no phase index (`1-spec/CLAUDE.spec.md`, `2-design/CLAUDE.design.md`, all `3-code/<component>/CLAUDE.component.md` files, `4-deploy/CLAUDE.deploy.md`) still references the retired decision as active.
