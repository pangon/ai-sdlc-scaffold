# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with this repository.

## Language Policy

**All AI outputs must be in English**, regardless of the language used in user prompts. This applies to code, comments, documentation, configuration files, commit messages, and response text.

---

## Project Overview

<!-- Replace this section with a description of your project. -->

This repository uses a structured, AI-first development lifecycle. All project knowledge — objectives, design, decisions, tasks — lives alongside the source code. See [README.md](README.md) for the full directory layout.

---

## Phase-Specific Instructions

Each phase directory contains a `CLAUDE.<phase>.md` file. When working in a phase:

1. Read the phase-specific instructions — they extend (not override) this file
2. Consult the decisions index in that phase file before starting work
3. Work within the appropriate phase structure

| Phase | Directory | Focus |
|-------|-----------|-------|
| **Objectives** | `1-objectives/` | Define what to build and why |
| **Design** | `2-design/` | Define how to build it |
| **Code** | `3-code/` | Build it |
| **Deploy** | `4-deploy/` | Ship and operate it |

### Phase Gates

Before creating artifacts in the next phase, check these minimum preconditions. Gates are advisory — warn the user if not met, but proceed if they confirm.

| Transition | Preconditions |
|------------|---------------|
| Objectives → Design | At least one goal Approved; at least one requirement Approved; stakeholders defined |
| Design → Code | `architecture.md` has content; at least one requirement has a corresponding task in `tasks.md` |
| Code → Deploy | At least one task Done |

---

## Artifact Status Lifecycle

All artifacts with a Status field (user stories, requirements, assumptions) follow this lifecycle:

```
Draft --[human approves]--> Approved --[linked task Done]--> Implemented
  \                            \
   +--[human decides]---------> Deprecated
```

**Transition rules:**
- **Draft → Approved**: only a human can approve (after stakeholder review)
- **Approved → Implemented**: the agent marks this when all linked tasks reach Done
- **Any → Deprecated**: only a human can deprecate; the agent proposes and waits for approval

---

## Graduated Safeguards

AI agents operate autonomously within development tasks. For project-level decisions, the scaffold defines three tiers:

| Tier | When | Agent behavior |
|------|------|----------------|
| **Always ask** | Conflict resolution, design gaps, decision deprecation/supersession, phase gate advancement | Stop, present options, wait for human approval |
| **Ask first time, then follow precedent** | Naming conventions, error handling patterns, test structure | Ask once, record the decision, apply consistently afterward |
| **Decide and record** | Routine implementation choices within established patterns | Decide autonomously, record in the appropriate artifact |

When spotting a related issue, potential improvement, or ambiguous situation during a task, **surface it to the user** instead of silently deciding to act or not act.

---

## Decisions

All project decisions live in `2-design/decisions/`. Each decision consists of two files:

- **`DEC-NNN-short-name.md`** — the active record: context, decision, and enforcement rules. Read during normal task execution.
- **`DEC-NNN-short-name.history.md`** — the trail: alternatives, reasoning, human involvement, changelog. Read only when evaluating or changing a decision.

**Naming**: `NNN` is a three-digit sequential number; `short-name` is kebab-case.

**Templates**: [`2-design/decisions/_template.md`](2-design/decisions/_template.md) and [`_template.history.md`](2-design/decisions/_template.history.md).

Phase-specific decision indexes (with trigger conditions) are in each `CLAUDE.<phase>.md`. A decision may appear in multiple phase indexes.

### Agent navigation rules

- **Normal execution**: consult the decisions index in the current phase → read relevant `DEC-NNN.md` files → apply enforcement.
- **Evaluating or changing a decision**: read `DEC-NNN.md`, then `DEC-NNN.history.md` → propose changes → update both files → append to changelog.
- **Never modify** `*.history.md` except to append to the changelog or add alternatives when proposing a supersession.

### Human involvement vocabulary

| Value | Meaning |
|-------|---------|
| `human-decided` | Human made the decision; AI had no significant role |
| `human-argued` | Human proposed and argued the rationale explicitly |
| `collaborative` | Human and AI jointly reasoned through options |
| `ai-proposed/human-approved` | AI proposed; human explicitly approved |
| `ai-proposed/auto-accepted` | AI proposed and recorded without explicit human approval |

### When Recording Decisions

When a significant decision, pattern, or constraint emerges:

1. Create `DEC-NNN-short-name.md` from [`_template.md`](2-design/decisions/_template.md) and fill in all fields.
2. Create `DEC-NNN-short-name.history.md` from [`_template.history.md`](2-design/decisions/_template.history.md) and fill in all fields.
3. Add an entry to the decisions index of every phase whose trigger conditions are met.

### Deprecating or Superseding a Decision

A decision should be deprecated when no longer relevant, or superseded when a new decision replaces it.

**Never deprecate or supersede silently.** Always surface the proposal to the user first.

1. **Identify the candidate**: note the DEC-NNN ID and reason for retirement.
2. **Read both files**: `DEC-NNN.md` and `DEC-NNN.history.md` to understand full context.
3. **Ask the user.** Present:
   - Why the decision is no longer valid or should be replaced
   - Whether existing code, infrastructure, or process still depends on it
   - The proposed action: deprecate (retire) or supersede (replace with new decision)
4. **Wait for explicit approval** before modifying any file.
5. **Apply:**

   **If deprecating:**
   - In `DEC-NNN.md`: change `**Status**` to `Deprecated`.
   - In `DEC-NNN.history.md`: append a changelog entry with date, change, and involvement type.
   - Remove the decision from every phase index.

   **If superseding:**
   - Create the replacement decision (`DEC-MMM`) following the recording procedure.
   - In old `DEC-NNN.md`: change `**Status**` to `Superseded by DEC-MMM`.
   - In old `DEC-NNN.history.md`: append changelog entry.
   - In every phase index: replace the old row with the new one.

6. **Verify**: no phase index still references the retired decision as active.

---

## After Making Changes

Evaluate whether to:

1. **Update this file** if project-wide patterns or architecture change significantly.
2. **Update phase-specific files** (`CLAUDE.<phase>.md`) if phase-specific patterns or conventions are established.
3. **Create new instruction files** if a workflow becomes complex enough to need dedicated guidance.

Proactively suggest these updates when relevant.
