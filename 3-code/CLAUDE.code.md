Phase-specific instructions for the **Code** phase. Extends [../CLAUDE.md](../CLAUDE.md).

## Purpose

This phase contains the **implementation**. Focus on clean, tested, maintainable code.

---

## Components

<!-- Add an entry for each component/codebase -->

---

## Component Isolation

All source code, configuration, and assets for a component **must reside within that component's directory**. Specifically:

- **No code outside component directories** — never place source files, configuration files, or build artifacts in `3-code/` itself or anywhere else outside the owning component's directory.
- **No cross-component configuration** — configuration that spans multiple components should never be necessary. If such a situation arises, treat it as a potential design flaw or incorrect component separation. Stop work, notify the user with a clear description of the conflict, and propose alternative actions (e.g., refactoring responsibilities, introducing a new component, or adjusting the design).
- **Do not rename or move component directories** — the directory names listed above are fixed; renaming or relocating them breaks cross-phase references and tooling assumptions.

---

## Build Commands

Scripts and commands for each component are documented in that component's own codebase (package.json, Makefile, README, or equivalent). Check there first.

When invoking any command, apply active decisions from the component's `CLAUDE.component.md` whose trigger conditions match.

---

## Task Tracking

All development tasks are tracked in [`tasks.md`](tasks.md).

To create the initial implementation plan (phased tasks from design artifacts), run `/SDLC-implementation-plan`. This should be done after `/SDLC-decompose` and before starting any coding work.

---

## Implementation Logs

Each task execution and each fix execution produces a durable log under [`implementation-log/`](implementation-log/). There is one log file per task or fix:

- **Task logs** — file name `<TASK-ID>.md` (e.g., `TASK-model-service-list.md`). Created and maintained by the `SDLC-execute-task` skill.
- **Fix logs** — file name `<FIX-ID>.md` (e.g., `FIX-asset-list-pagination-off-by-one.md`). Created and maintained by the `SDLC-fix` skill.

Each log captures the synthesized understanding of the task or fix followed by a chronological append-only record of non-trivial operations and evaluations performed during execution (writes, questions to the user, reconsiderations, problems, in-flight corrections, design gaps, tensions, test outcomes).

---

## Linking to Other Phases

- Implementation follows designs in `2-design/`
- Tests verify requirements from `1-spec/`
- Infrastructure code goes in `4-deploy/`; when a coding task modifies IaC, the deploy phase instructions ([`CLAUDE.deploy.md`](../4-deploy/CLAUDE.deploy.md)) apply as well
