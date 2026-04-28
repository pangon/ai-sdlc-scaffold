---
name: SDLC-execute-task
description: Execute a development task from the implementation plan. Selects the next pending task when invoked bare, or uses user-provided context to identify the task otherwise. Derives actual scope from specification/design/decisions, implements with tests, handles design gaps, and updates task status. Use during the Code phase.
---

## Instructions

You are executing a development task from the implementation plan. You will identify the task, derive its actual scope from the authoritative sources (requirements, design documents, decisions), implement it (or decompose it if too large), and update the task tracker.

### Invocation Modes

This skill has two invocation modes, determined by whether the user provided context or instructions alongside the skill invocation:

- **Bare invocation** — the skill was invoked with no additional context, arguments, or instructions (e.g., just `/SDLC-execute-task` on its own). In this mode, the skill automatically selects the **next pending task** from the implementation plan following the rules in "Task Selection — Bare Invocation" below.
- **Contextual invocation** — the skill was invoked together with context or instructions (a specific task ID, a natural-language description of which task to work on, scoping hints like "the auth task in phase 2", or any other directive narrowing the target). In this mode, the skill uses that context to identify which task to execute, following the rules in "Task Selection — Contextual Invocation" below. The user's context takes precedence: do **not** silently fall back to selecting the next task if the context is ambiguous — ask the user to clarify instead.

Determine the mode before doing anything else, and follow the corresponding Task Selection branch.

### Task Descriptions Are Indicative, Not Prescriptive

The `Task` column in `tasks.md` contains a **brief, indicative description** — a shorthand hint of what the task is about. It is not a specification and must not be followed literally when doing so would conflict with the authoritative sources or the project's goals.

**The authoritative sources for what a task must deliver are, in order of precedence:**

1. **Requirements** (linked in the `Req` column) — the acceptance criteria define what "done" means.
2. **Design documents** (`2-design/`) — the architecture, data model, and API design define *how* the system is structured.
3. **Applicable decisions** (`DEC-*` files) — recorded patterns and constraints that must be respected.
4. **SDLC system instructions** (`CLAUDE.md`, phase-specific `CLAUDE.*.md` files) — project-wide rules and conventions.
5. **Phase capabilities** (the "Capabilities delivered" section in the Execution Plan) — what the current phase is supposed to make possible.
6. **Downstream task needs** (tasks that list this task in their `Dependencies` column) — what other tasks will need from the output of this one.

The task description serves as an **orientation aid** — it tells you roughly which area of the system to work in and what kind of work to do. But the actual scope, behavior, and implementation details must be derived from the sources above.

A mismatch between the task description and the authoritative sources is **not** a design gap — it simply means the brief description was imprecise.

### Phase Validation

Before doing anything else, read the `### Current State` subsection under `## Project Overview` in `CLAUDE.md` and determine which phase the project is in. Then follow the matching case below:

1. **Project not initialized** — the Current State lacks a real project description (e.g., mentions "not yet been initialized" or "base scaffold"). **Stop**, recommend `/SDLC-init`, and do not proceed.

2. **Project is in the Specification phase** — the Current State mentions "Specification phase", lists specification artifacts being drafted, or no phase beyond Specification has been started. **Stop**, recommend `/SDLC-elicit` to continue refining specification or `/SDLC-design` to start the design phase, and do not proceed.

3. **Project is in the Design phase** — **Stop**, recommend completing design with `/SDLC-design`, then `/SDLC-decompose` and `/SDLC-implementation-plan` to create the task list, and do not proceed.

4. **Project is in the Code phase but has no tasks** — `3-code/tasks.md` has no task entries (only template/legend). **Stop**, recommend `/SDLC-implementation-plan` to create the task list, and do not proceed.

5. **Project is in the Code phase with tasks** — this is the expected state. Proceed with Task Selection.

### Task Selection — Bare Invocation

Use this branch only when the skill was invoked with no additional user context (see "Invocation Modes" above).

1. **Read code phase instructions**: Open `3-code/CLAUDE.code.md`.

2. **Read the task list**: Open `3-code/tasks.md`.

3. **Read the Execution Plan**: Look at the Execution Plan section to understand phase ordering. Phases are executed sequentially — complete all tasks in a phase before moving to the next.

4. **Check for interrupted tasks**: Scan the Task Table for any task with status `In Progress`. If one is found, **stop** — that task was likely interrupted in a previous session. Inform the user which task is in progress and ask how they want to proceed (resume, reset to Todo, or cancel it) before continuing. Do not select a different task.

5. **Find the current phase**: Identify the earliest phase that still has tasks with a status other than `Done` or `Cancelled`. If no such phase exists:
   - If any tasks across all phases have status `Blocked`, **stop** and inform the user that the implementation plan is complete except for blocked tasks. List the blocked tasks and their blockers. Do not proceed.
   - If no tasks are blocked either, **stop** and inform the user that the entire implementation plan has been completed. Do not proceed.

6. **Unblock eligible tasks**: Scan the current phase for tasks with status `Blocked`. For each blocked task:
   - Read the `Notes` column to understand why it was blocked.
   - Check the status of the tasks it depends on (from the `Dependencies` column).
   - If the blocking condition described in `Notes` no longer applies and all dependencies are `Done`, update its status to `Todo` and set `Updated` to today's date.

7. **Find the next task**: Within the current phase, find the first task (in the listed order) that has status `Todo` in the Task Table.

8. **Validate dependencies**: Check that all tasks listed in the selected task's `Dependencies` column are marked as `Done`.
   - If any dependency is **not** `Done`, **stop** — inform the user that this task has unmet dependencies and should likely be marked as `Blocked`. List the unmet dependencies and ask the user how they want to proceed (mark it as Blocked, skip it, force-start it, execute one of the blocking tasks first, or revise the implementation plan). If the user chooses to mark the task as `Blocked`, record the blocking reason in the `Notes` column (e.g., "Blocked: waiting on TASK-xyz which is still Todo").
   - If all dependencies are `Done`, select this task for execution and proceed to Task Preparation.

### Task Selection — Contextual Invocation

Use this branch only when the skill was invoked with user-provided context (see "Invocation Modes" above).

1. **Read code phase instructions**: Open `3-code/CLAUDE.code.md`.

2. **Read the task list**: Open `3-code/tasks.md`.

3. **Resolve the user's context to a single task**:
   - If the user gave an exact task ID (e.g., `TASK-foo-bar`), look it up in the Task Table.
   - Otherwise, match the user's description against task IDs, task descriptions, phases, and components in the Task Table.
   - If exactly one task matches, select it.
   - If multiple tasks plausibly match, or no task matches, **stop** and ask the user to disambiguate. List the candidates (or the closest matches) with their IDs, phases, and descriptions. Do **not** fall back to the next task automatically — the user's intent must be honored.

4. **Check for interrupted tasks**: If any task in the Task Table has status `In Progress` and it is **not** the one the user just selected, inform the user that another task is mid-flight and ask whether to continue with the user-selected task anyway, switch to finishing the in-progress task, or cancel. Proceed according to the user's answer.

5. **Check the selected task's status**:
   - `Done` or `Cancelled`: **stop** and inform the user — the task is not actionable. Ask whether they want to reopen it (e.g., reset to `Todo`) or pick a different one.
   - `In Progress`: treat this as resuming an interrupted task — proceed to Task Preparation.
   - `Blocked`: read the `Notes` column and check the task's `Dependencies`. Surface the reason to the user and ask whether to force-start it, unblock (if the blocker is no longer valid — then set status to `Todo`), or pick a different task.
   - `Todo`: proceed to step 6.

6. **Validate dependencies**: Check that all tasks listed in the selected task's `Dependencies` column are marked as `Done`.
   - If any dependency is **not** `Done`, surface this to the user and ask how to proceed (force-start, execute a blocking task first, pick a different task, or revise the plan). Do not auto-mark as `Blocked` — the user explicitly asked for this task, so require their decision.
   - If all dependencies are `Done`, select this task for execution and proceed to Task Preparation.

7. **Respect additional user scoping**: if the user's context included instructions that narrow or adjust the work within the task (e.g., "do only the migration part", "skip the tests for now", "use library X"), carry those instructions forward into Task Preparation and Execution — they override the default behaviors where they conflict (but they do **not** override authoritative sources; if the user's instruction conflicts with a requirement, design document, or decision, treat it as a tension per Task Preparation step 5).

### Task Preparation

Before writing any code, gather all necessary context.

**Resume branch.** If an implementation log already exists at `3-code/implementation-log/<task-id>.md` (a prior skill run worked on this task), Task Preparation is grounded in that log:

- Read the log in full **before** starting the steps below. Pay particular attention to `[Q&A]` entries (questions the user already answered — do not ask them again), `[RECONSIDER]` entries (earlier plan changes that shape the current approach), and every `[CONCLUSION]` entry present (prior outcomes, open items, and any reason the task was reopened).
- Treat the log's `## Understanding` section as the **authoritative synthesized scope** for this task. Do **not** re-derive it, and do **not** edit it — even if the original synthesis now feels incomplete. If your reading of the sources produces a different understanding, capture the delta as a `[RECONSIDER]` entry later in the Execution log; never overwrite the original Understanding.
- Still execute all of steps 1–8 below, but informed by the log. In particular, for steps 1–3: ensure every artifact directly referenced in the log (linked requirements, design documents, decisions, downstream tasks, plus anything cited inside `[Q&A]` / `[RECONSIDER]` / `[CONCLUSION]` entries) is re-read and understood, even if it was not originally listed in the Understanding section. Use these steps to verify and supplement the existing Understanding, not to replace it.
- For steps 4–8 (applicable decisions, constraint tensions, dev-env needs, confirm understanding, complexity), also run them against the **current** state of the project, since decisions, constraints, or the environment may have drifted since the prior run.

**Fresh branch.** If no log file exists, run all of steps 1–8 below to derive the Understanding from scratch; it will be written into the log during Implementation Log setup.

#### 1. Read Requirements

Read every requirement listed in the task's `Req` column (follow the links in the Task Table). Understand what the task must deliver, including acceptance criteria. The acceptance criteria in the requirements are the primary measure of "done" — not the task description.

If the task has no linked requirements (`-` in the Req column), the task is infrastructure or enablement work. In that case, its purpose is to make downstream tasks possible — proceed to step 2 to understand what downstream tasks need.

#### 2. Understand the Task's Role in the Plan

Go beyond the task description to understand the task's **actual purpose** in the development plan:

- **Read the phase's "Capabilities delivered"** section in the Execution Plan. Understand what capability this task contributes to.
- **Identify downstream tasks** — scan the Task Table for tasks that list this task in their `Dependencies` column. Read their descriptions and linked requirements to understand what they will need from this task's output (data structures, interfaces, endpoints, modules, etc.).
- **Synthesize the intent**: combining the linked requirements, the phase capabilities, and downstream needs, form a clear understanding of what this task must produce. This synthesized intent takes precedence over the literal task description.

#### 3. Review Design Documents

Read relevant design documents in `2-design/` — at minimum the ones that cover the area being implemented (architecture, data model, API design). Understand the intended design for this task.

#### 4. Check Applicable Decisions

Read the `CLAUDE.component.md` file for the component this task belongs to (determined by which Task Table section it appears in). Review the `## Relevant Decisions` table:
- Scan the table and identify any decisions whose trigger conditions match the current task.
- Read the full decision file(s) for applicable decisions (follow the File column links).
- These decisions **must** be applied during implementation.

Additionally, if the task involves multiple components, also read the `CLAUDE.component.md` file for each involved component and review their `## Relevant Decisions` tables — decisions from all involved components must be considered and applied. For tasks in the `Setup & Infrastructure` or `Deploy & Operations` sections, also check the decisions indexes in `2-design/CLAUDE.design.md` and `4-deploy/CLAUDE.deploy.md` respectively.

#### 5. Check for Constraint Tensions

After gathering all context (requirements, design documents, decisions, phase instructions), actively look for **tensions between authoritative sources**. A tension exists when two or more sources that the agent must follow pull in incompatible directions — satisfying one fully would require violating or bending another.

Common tension patterns:
- **Architectural decisions vs. component isolation rules** — e.g., a decision requiring shared state across components while isolation rules prohibit cross-component coupling.
- **Cross-cutting decisions vs. per-component conventions** — e.g., a system-wide decision that conflicts with a component-specific pattern.
- **Requirements vs. constraints** — e.g., a functional requirement that is difficult to satisfy within a declared constraint.
- **Design document assumptions vs. phase instructions** — e.g., a design that assumes a project structure incompatible with the code phase conventions.

**If a tension is found, stop and surface it to the user before implementing.** Present:
1. **The conflicting sources** — cite both by name and quote the relevant rules.
2. **Why they conflict** — explain the specific scenario where following one source requires bending the other.
3. **Options** — propose at least two resolution paths (e.g., adjust one source, record a scoped exception as a decision, restructure the approach).
4. **Wait for the user's decision** before proceeding.

A tension is **not** the same as a design gap (divergence between design and implementation needs). Tensions are contradictions *within* the set of authoritative sources. They are an "always ask" tier situation — the agent must never resolve them silently, because choosing one source over another is a project-level decision that belongs to the user.

If no tensions are found, proceed to step 6.

#### 6. Evaluate Development Environment Needs

Assess whether this task requires **development environment setup or interaction** — runtime isolation, version management, package installation, build tooling, test infrastructure, or similar concerns.

**When this step applies:**
- The task creates or modifies project scaffolding (package manifests, lockfiles, build configuration)
- The task installs, upgrades, or removes dependencies
- The task sets up or configures test infrastructure (test runners, coverage tools, assertion libraries, fixtures)
- The task runs build commands, test runners, linters, or other tooling
- The component's tech stack has well-known environment isolation conventions (e.g., Python virtual environments, Node version managers, Ruby gemsets, Rust toolchains)

**When this step does NOT apply:**
- The task only edits source code without running any commands
- The environment is already established and documented (e.g., a prior task created and documented the venv, and its activation path is recorded in a decision or component instructions)

**If it applies, check in order:**

1. **Look for an existing environment convention** — check the decisions already identified in step 4 for any that define environment setup conventions (e.g., a `DEC-python-venv` or `DEC-node-version-manager`). If no applicable decision exists, fall back to checking the component's `README`, `Makefile`, or equivalent files in the component directory for documented conventions. If a convention is found by either route (e.g., "use `.venv` in the component directory", "use `nvm` with `.nvmrc`"), follow it.

2. **If no convention exists and this task establishes one** — identify the ecosystem's standard practices for the tech stack across all relevant areas:
   - **Runtime isolation**: e.g., Python → virtual environment; Node.js → version manager + lockfile; Rust → `rustup` toolchain; Go → module-aware mode.
   - **Test infrastructure**: e.g., Python → pytest + configuration in `pyproject.toml`; Node.js → Vitest/Jest + config; Rust → built-in `cargo test`; Go → built-in `go test`.
   **Stop and present the choice to the user** before proceeding:
   - State which environment practices you intend to adopt and why they are standard for the stack.
   - Propose specifics (tool, location, configuration files).
   - Wait for confirmation.
   - After confirmation, record the convention as a decision (`DEC-*`) so future tasks follow it automatically (step 1 above).

3. **If an environment exists but the task modifies it** (e.g., adding dependencies, changing runtime version) — ensure commands run *within* the established environment (e.g., activate the venv before `pip install`, use the project's Node version before `npm install`).

#### 7. Confirm Understanding

After completing steps 1–6, you should have a clear picture of what this task must deliver, derived from the authoritative sources — not just from the task description. Assess whether you have a clear and reasonably certain understanding of the work required.

If, despite having read the requirements, design documents, applicable decisions, and downstream task needs, the task's scope, expected behavior, or implementation approach remains ambiguous — **stop and ask the user** for confirmation or additional information before proceeding. Do not guess or assume intent when genuine uncertainty exists.

If your synthesized understanding of what the task requires diverges significantly from its brief description, **briefly note this to the user** (e.g., "The task description says X, but the requirements and design indicate the actual scope is Y — proceeding with Y"). This is informational, not a blocker — proceed unless you need user input on genuine ambiguity.

#### 8. Evaluate Task Complexity

Assess whether the task is too large to complete in one session. A task is "too large" if it would require:
- Multiple distinct components that could be done separately
- Complex logic that should be tested incrementally
- More code than can be reasonably reviewed at once

### Implementation Log

Every task execution must produce a durable log of the non-trivial operations and evaluations made during the run. The log is the paper trail a future reviewer or agent reads to understand what was done and why. The log is **append-only**: once the Understanding section has been populated and an entry has been written to the Execution log, they are never edited or removed — new entries are only appended.

#### Location and naming

- Logs live under `3-code/implementation-log/`.
- One file per task. File name is the task ID plus `.md` (e.g., `3-code/implementation-log/TASK-model-service-list.md`).
- The template is `3-code/implementation-log/_template.md`. Always start a new log file from this template — never from scratch.

#### Log file setup

Before entering the Execution section, do the following in order:

1. **Check whether the log file exists** at `3-code/implementation-log/<task-id>.md`.

2. **If the file does NOT exist:**
   - Copy `_template.md` to `<task-id>.md`.
   - Fill the header metadata: task ID + link and today's date as `Started`. The header has no other mutable fields — task status lives in `tasks.md`, and the final outcome will be captured as a `[CONCLUSION]` entry appended at the end.

3. **If the file DOES exist:**
   - The task's status in `tasks.md` **must** be `In Progress` — a pre-existing log implies a prior skill run on the same task (either interrupted, or concluded and now reopened). If the status is anything else (`Todo`, `Done`, `Cancelled`, `Blocked`), **stop** and surface the inconsistency to the user. Likely causes: a previous run crashed before updating status, the log file was created manually, the task was reset without cleaning up the log, or a stale log was left in place. Ask how to proceed (resume by setting the status to `In Progress`, archive/delete the log and restart from template, or investigate further). Do not proceed until the user resolves the inconsistency.
   - If the status is `In Progress`, the log should have already been read in full during Task Preparation's Resume branch (including its `## Understanding` section and its `[Q&A]`, `[RECONSIDER]`, and `[CONCLUSION]` entries). Append a `[RESUMED]` entry to the `## Execution log` section noting today's date and a brief indication of what triggered the resumption, then continue. Do not modify the pre-existing header, Understanding, or any earlier entry.

4. **Populate the Understanding section** (only on the fresh branch — skip this step when resuming a pre-existing log): fill the `## Understanding` section of the log with the synthesized scope derived during Task Preparation. Include hyperlinks to every referenced artifact — linked requirements, relevant design documents, applicable decisions, and downstream tasks. If the synthesized scope diverges from the brief description in `tasks.md`, note the divergence explicitly. Once filled, this section is immutable for the lifetime of the task — if new understanding emerges later, record it as a `[RECONSIDER]` or `[NOTE]` entry in the Execution log instead.

Only after the log file is in place and the Understanding section has been populated may you proceed to the Execution section.

#### What to log during Execution

The goal is to preserve the **important operations** and the **important evaluations** the skill made, not to transcribe everything. Append entries to the `## Execution log` section as the work progresses — not only at the end — so that, if the run is interrupted, the log still reflects what happened up to that point. Never edit or delete an existing entry; if a previous entry turns out to be wrong or superseded, append a new `[RECONSIDER]` or `[NOTE]` entry that references and corrects it.

**Log:**

- **Every file write, create, or delete** as a `[WRITE]` entry — include the path and a one-line summary of what changed and why. Do not paste full diffs.
- **Every question asked to the user** as a `[Q&A]` entry — include both the question and the user's answer (verbatim or faithfully paraphrased).
- **Every reconsideration or mid-flight change of plan** as a `[RECONSIDER]` entry — what shifted and why.
- **Every problem detected** as a `[PROBLEM]` entry, and the corresponding fix as a `[FIX]` entry referencing it.
- **Every design gap** surfaced via the Design Gap Procedure as a `[DESIGN-GAP]` entry — include the resolution (design-document update, new decision, deviation accepted, etc.).
- **Every tension between authoritative sources** surfaced to the user as a `[TENSION]` entry — include the resolution.
- **Test-run outcomes worth preserving** (failures with root cause, debug iterations, the final green run) as `[TEST]` entries.
- **Any other important observation or evaluation** as a `[NOTE]` entry.

**Do not log:**

- Routine file reads (requirements, design docs, decisions — these are implicit context).
- Routine tool invocations that carry no decision weight.
- Verbatim long tool output — summarize instead.

#### Finalizing the log

When the skill finishes — Path A success (task `Done`), Path B decomposition, or stopping with `Blocked` / `Cancelled` — append a `[CONCLUSION]` entry as the last entry of the Execution log. Its body must state the final status and summarize: files created/modified, tests added/updated, requirements and acceptance criteria satisfied, new or updated decisions, design-document updates, and any pre-existing issues observed.

A log may accumulate multiple `[CONCLUSION]` entries across its lifetime: if a task that already has a `[CONCLUSION]` is reopened (for instance, the user sets its status back to `In Progress` and re-invokes the skill), the resumed run appends a fresh `[RESUMED]` entry and, when it finishes, a new `[CONCLUSION]` entry. Prior `[CONCLUSION]` entries are never edited or removed — the latest one reflects the current state, the earlier ones remain as historical record.

### Execution

Follow one of two paths based on complexity assessment. Throughout both paths, maintain the implementation log according to the rules in the "Implementation Log" section above — append entries as work progresses, not only at the end.

#### Path A: Task is Manageable — Execute

1. **Update status**: Set the task's Status to `In Progress` and update the `Updated` column with today's date in `tasks.md`.

2. **Check for design gaps before coding**: If you anticipate a significant divergence from the design documents (not from the task description — see the distinction in "Task Descriptions Are Indicative"), **stop and follow the Design Gap procedure** (below) before writing implementation code.

3. **Implement**:
   - Respect the component isolation rules in `3-code/CLAUDE.code.md`.
   - Write clear, self-documenting code following language/framework conventions and established best practices (e.g., SOLID principles, DRY, separation of concerns, meaningful naming, proper error handling, security best practices).
   - Prefer splitting code across multiple files over keeping a single large file, when compatible with language/framework conventions.
   - Keep functions small and focused.
   - Add comments only where logic isn't self-evident — do not add redundant or obvious comments.
   - Use strict type checking where available.
   - Apply all relevant decisions identified in Task Preparation step 4.

4. **Write tests**:
   - **Acceptance criteria coverage**: for each requirement linked in the task's `Req` column, review its acceptance criteria. Write tests that verify every acceptance criterion that is enabled by this task's implementation.
   - **Test organization**: follow the testing best practices and conventions of the project's tech stack (directory structure, naming, test runner configuration, fixtures, etc.).
   - **Unit tests**: write unit tests for new functionality — use descriptive test names that explain the scenario being tested.
   - **Integration tests**: if the task involves communication between different components, or between different modules within the same component, write integration tests in addition to unit tests.
   - **Bug fix tests**: when fixing a bug: (a) write a failing test that reproduces the bug before making any fix, (b) fix the bug and verify the test passes, (c) search the same component's codebase for the same bug pattern elsewhere — if found, **ask the user** whether to fix those occurrences too.

5. **Run tests and verify**:
   - Run the full test suite (or at minimum, all tests related to the current task and its component).
   - **All tests must pass** before the task can be considered complete.
   - If tests fail, analyze the failure, fix the issue, and re-run.
   - If after **3 consecutive fix-and-rerun iterations** tests still fail, **stop** — explain to the user what is failing, what you have tried, and ask how they want to proceed (continue debugging, adjust the approach, simplify the tests, or skip and mark the task as blocked).

6. **Check for design gaps after coding**: After implementing, evaluate whether any divergence from the design documents occurred and follow the Design Gap procedure if needed.

7. **Evaluate new decisions**: After completing the task, assess whether a new implementation pattern or convention emerged that should be documented:
   - Did you make a non-obvious implementation choice that future tasks should replicate?
   - Did you establish a pattern (error handling, UI interaction, data flow, naming) that should be consistent across the codebase?
   - If yes: record a new decision.
   - If no new decision is needed, skip this step.

8. **Update status**: Set the task's Status to `Done` and update the `Updated` column with today's date in `tasks.md`.

9. **Do NOT commit automatically** — leave changes for user review.

#### Path B: Task is Too Large — Decompose

1. **Do NOT execute the task.**
2. **Break it down** into smaller subtasks, each representing a focused unit of work.
3. **Insert the subtasks** into the Task Table in the appropriate component section, with:
   - **ID**: use the parent task ID with a descriptive suffix (e.g., `TASK-model-service-list`, `TASK-model-service-download`).
   - **Priority**: inherit from parent task.
   - **Status**: `Todo`.
   - **Req**: distribute the parent's requirement links to the relevant subtasks.
   - **Dependencies**: set correctly (subtasks may depend on each other or on the same dependencies as the parent).
   - **Updated**: today's date.
   - **Notes**: reference the parent task (e.g., "Split from TASK-model-service").
4. **Update the Execution Plan** to replace the parent task with the subtasks in the same phase, preserving the correct execution order.
5. **Set the parent task's status** to `Cancelled` with a note explaining the decomposition (e.g., "Decomposed into TASK-model-service-list, TASK-model-service-download, TASK-model-service-load").
6. **Explain** to the user what subtasks were created and why.

### Design Gap Procedure

A design gap is a divergence between **design documents** and what implementation requires. When updating design artifacts as part of this procedure, follow the procedures and instructions in `.claude/skills/SDLC-design/SKILL.md` — in particular the **Modifying Existing Design Documents** section (downstream effect checks, present changes, wait for confirmation), **Decision Triggers** (record new decisions when design choices are approved), **Current State Tracking** (update `CLAUDE.md`), and the cross-skill artifact procedures defined in `CLAUDE.md`.

**Minor divergence** (field renamed, type made more specific, optional field added): update the relevant `2-design/` file following the SDLC-design modification procedures, continue.

**Significant divergence** (new endpoint/entity, invalid architectural assumption, requirement that cannot be implemented as designed):

1. **Stop** — do not write implementation code.
2. **Surface** the gap: what the design says, what implementation needs, why they differ.
3. **Present options**: update design first, simplify to stay within design, or accept and record deviation as a decision.
4. **Wait for explicit approval** before proceeding.
5. **Act**: update `2-design/` or `1-spec/` as needed following the SDLC-design procedures (including downstream effect checks, decision recording, and Current State tracking), then implement.

### Current State Tracking

Whenever the skill completes a task (status set to `Done`) or decomposes a task, update the `### Current State` subsection under `## Project Overview` in `CLAUDE.md` to reflect:

1. **Task progress** — note overall progress (e.g., "Implementation progress: 5/42 tasks done, currently in Phase 2"). Update this after every task completion or decomposition.
2. **Design gap resolutions** — if the Design Gap Procedure was triggered and resulted in design document updates or new decisions, reflect those changes in the Current State (following the same conventions as the SDLC-design skill's Current State Tracking: update design document status, decision count, and mark completeness assessment as stale if applicable).

If a task execution only changes task status (no design impact), update only the task progress line. Do not rewrite the entire Current State section — update incrementally.

### Interaction Style

- After completing Task Preparation and before creating the log, briefly state which task was selected, its linked requirements, and applicable decisions. If your synthesized understanding of the task's scope differs from its brief description, note the actual scope you will implement. Do not ask for permission to begin unless genuine ambiguity exists (see Task Preparation step 7).
- **The implementation log is the gate into Execution, not a closing artifact.** After the briefing and before any Execution-path action — flipping the task's status to `In Progress`, writing product files, running tests, editing `tasks.md` or `CLAUDE.md`, proposing new decisions — create `3-code/implementation-log/<task-id>.md` from the template and populate its `## Understanding` section in full, reusing the synthesis you just briefed. Then enter Execution, appending entries (`[WRITE]` / `[Q&A]` / `[RECONSIDER]` / `[PROBLEM]` / `[FIX]` / `[DESIGN-GAP]` / `[TENSION]` / `[TEST]` / `[NOTE]`) as work progresses — never batched at the end — and close with a `[CONCLUSION]`. Full rules live in the "Implementation Log" section above.
- During implementation, work autonomously — do not ask for confirmation at every step. The stop-and-ask points are explicitly defined in the instructions (interrupted tasks, unmet dependencies, design gaps, ambiguity).
- **After completing a task (executed or decomposed), report the outcome and ask the user how they want to proceed** — e.g., review changes, commit, or stop. Do not automatically start the next task.
- When a design gap is found, present it clearly with context, options, and trade-offs. Do not minimize the gap or push toward a specific resolution.
- When decomposing a task, explain the rationale for the split and how the subtasks relate to each other.

### Rules

- **Read-before-write**: always read existing files before proposing changes.
- **Requirements first**: read every linked requirement before writing code. Do not implement from memory or assumption.
- **Log is mandatory and append-only**: the per-task implementation log under `3-code/implementation-log/<task-id>.md` must exist and contain the Understanding section before the Execution section starts, and new entries (`[WRITE]` / `[Q&A]` / `[RECONSIDER]` / `[PROBLEM]` / `[FIX]` / `[DESIGN-GAP]` / `[TENSION]` / `[TEST]` / `[NOTE]`) must be appended as work progresses. Existing content is never edited or removed — corrections are appended as new entries. Each run of the skill terminates by appending a `[CONCLUSION]` entry; a reopened task will therefore accumulate multiple `[RESUMED]` / `[CONCLUSION]` pairs over time. A pre-existing log file with a task status other than `In Progress` is an inconsistency that must be surfaced to the user before continuing.
- **Decisions are mandatory**: applicable decisions from the component's `CLAUDE.component.md` must be followed. If a decision conflicts with the task, surface it to the user — do not silently ignore it.
- **Task list integrity**: never reorder the task list within a phase in `tasks.md`, and never rename pre-existing task IDs.
- **No auto-commit**: leave all changes for user review. Do not commit or push.
- **Traceability**: if the implementation reveals that a requirement is missing or incorrect, surface it to the user and recommend `/SDLC-elicit` to address it.
- **Deploy awareness**: if the task touches infrastructure as code (Terraform, Dockerfiles, CI/CD pipelines, deployment scripts, etc.), also read `4-deploy/CLAUDE.deploy.md`, review its decisions index, and follow its instructions alongside these.
- **External tool and library dependencies**: when a task requires tools or libraries that are not available in the current environment (system packages like ffmpeg, database engines, runtime tools, etc.), **always notify the user before attempting to install them**. If the user approves and installation fails (e.g., no sudo access, missing package manager), provide the user with clear instructions on how to install the dependency themselves and **wait for the user to confirm** that installation is complete before proceeding. Never silently install system-level dependencies or work around their absence without user awareness.

### Output

At the end, report:
- Which task was identified as next
- Whether it was executed or decomposed
- What was accomplished (describing actual scope delivered, which may differ from the task description)
- Which requirements were satisfied and which acceptance criteria were covered
- Which decisions were applied (if any)
- Whether a constraint tension was found between authoritative sources and how it was resolved (if any)
- Whether a design gap was found and how it was resolved (if any)
- Whether a new decision was proposed (if any)
- **Pre-existing issues observed** — if during implementation or testing you encountered problems in code **outside** this task's scope (e.g., type errors, linting warnings, test failures, deprecation warnings in pre-existing files), list them prominently under a dedicated heading (e.g., `### Pre-existing issues observed`). For each issue, state: (1) which file(s) and what the problem is, (2) why it was not fixed, and (3) a recommendation for when to address it (e.g., a dedicated cleanup task, the next task that touches those files, or immediately).
