---
name: SDLC-fix
description: Apply a user-reported fix, bug correction, or change to one or more components. Gathers context interactively, identifies affected components, builds informative context from design artifacts and decisions, implements the fix with tests, and handles design gaps. Use during the Code phase or later.
---

## Instructions

You are applying a user-reported fix, bug correction, or modification to the codebase. The work is **not** driven by `tasks.md` — the user describes what needs to be done and you derive the scope, context, and implementation plan from that description combined with the project's authoritative sources.

### Phase Validation

Before doing anything else, read the `### Current State` subsection under `## Project Overview` in `CLAUDE.md` and determine which phase the project is in. Then follow the matching case below:

1. **Project not initialized** — the Current State lacks a real project description (e.g., mentions "not yet been initialized" or "base scaffold"). **Stop**, recommend `/SDLC-init`, and do not proceed.

2. **Project is in the Specification phase** — the Current State mentions "Specification phase", lists specification artifacts being drafted, or no phase beyond Specification has been started. **Stop**, recommend `/SDLC-elicit` to continue refining specification or `/SDLC-design` to start the design phase, and do not proceed.

3. **Project is in the Design phase** — **Stop**, recommend completing design with `/SDLC-design`, then `/SDLC-decompose` and `/SDLC-implementation-plan` to create the task list, and do not proceed.

4. **Project is in the Code phase or later** — this is the expected state. Proceed with Issue Elicitation.

### Issue Elicitation

Gather enough context from the user to understand the problem and determine the scope of the fix. Ask the user the following, adapting the questions to what they have already provided:

1. **Problem description** — What is the issue, bug, or desired change? Ask for:
   - Observed behavior vs. expected behavior (for bugs)
   - What they want to achieve (for changes/enhancements)
   - Steps to reproduce (for bugs, if applicable)
   - Error messages or logs (if available)

2. **Affected area** — Which part(s) of the system are involved? Help the user identify:
   - Which **component(s)** are affected (e.g., frontend, backend) — refer to the components listed in `3-code/CLAUDE.code.md`
   - Which **scripts or runbooks** are involved (if the fix targets deploy/operations artifacts in `4-deploy/`)
   - Which **specific files, modules, or features** they believe are involved (if they know)

3. **Scope boundaries** — Clarify what is in scope and what is not:
   - Is this a minimal targeted fix, or should related issues in the same area also be addressed?
   - Are there any constraints on the approach (e.g., "don't change the API contract", "must be backwards compatible")?

**Do not proceed until you have a clear understanding of the problem and the affected area(s).** If the user's description is vague, ask follow-up questions. It is better to ask one or two clarifying questions than to implement the wrong fix.

Once you have sufficient context, summarize your understanding back to the user in a brief statement (e.g., "I understand the issue is X, affecting component Y, and the fix should Z. Proceeding.") and wait for confirmation.

#### Assigning the fix identifier

After the user confirms the understanding, derive a descriptive kebab-case slug from the elicited problem and form the fix ID as `FIX-<kebab-slug>` (e.g., `FIX-asset-list-pagination-off-by-one`). The fix ID is the permanent handle for this work and the filename of its implementation log. Follow the `PREFIX-kebab-name` artifact-naming convention from [`CLAUDE.md`](../../../CLAUDE.md). Propose the ID to the user, accept any adjustment they request, then check whether `3-code/implementation-log/<FIX-ID>.md` already exists:

- **No existing log** — this is a fresh fix. Proceed to Context Gathering on the fresh branch.
- **Existing log** — surface it to the user. Ask whether (a) this invocation **resumes** the prior fix (the new information supplements the existing record — proceed on the Resume branch), or (b) it is a **distinct** fix that happens to share a slug (in which case agree on a disambiguated slug with the user before proceeding). Do not proceed until the user answers.

If, during the initial elicitation, the user explicitly signalled that this is a continuation of a prior fix (e.g., by naming a FIX-ID or referencing a past attempt), use that ID here instead of deriving a new slug, and go straight to the existing-log branch above.

### Context Gathering

Before writing any code, gather all necessary context.

**Resume branch.** If an implementation log already exists at `3-code/implementation-log/<FIX-ID>.md` and the user confirmed during Issue Elicitation that this invocation resumes that fix, Context Gathering is grounded in that log:

- Read the log in full **before** starting the steps below. Pay particular attention to `[Q&A]` entries (questions the user already answered — do not ask them again), `[RECONSIDER]` entries (earlier plan changes that shape the current approach), and every `[CONCLUSION]` entry present (prior outcomes, open items, and any reason the fix was reopened).
- Treat the log's `## Understanding` section as the **authoritative synthesized scope** for this fix. Do **not** re-derive it, and do **not** edit it — even if the original synthesis now feels incomplete. If your reading of the sources produces a different understanding, capture the delta as a `[RECONSIDER]` entry later in the Execution log; never overwrite the original Understanding.
- Still execute all of steps 1–8 below, but informed by the log. In particular, for steps 1–5: ensure every artifact directly referenced in the log (linked requirements, design documents, decisions, plus anything cited inside `[Q&A]` / `[RECONSIDER]` / `[CONCLUSION]` entries) is re-read and understood, even if it was not originally listed in the Understanding section. Use these steps to verify and supplement the existing Understanding, not to replace it.
- For steps 6–8 (constraint tensions, dev-env needs, confirm approach), also run them against the **current** state of the project, since decisions, constraints, or the environment may have drifted since the prior run.

**Fresh branch.** If no log file exists, run all of steps 1–8 below to derive the Understanding from scratch; it will be written into the log during Implementation Log setup.

#### 1. Identify Affected Components

Based on the elicited information, determine which component(s) are affected. For each affected component:

- Read its `CLAUDE.component.md` file (e.g., `3-code/frontend/CLAUDE.component.md`, `3-code/backend/CLAUDE.component.md`).
- Note its technology stack, conventions, and directory structure.

If the fix targets deploy/operations artifacts, also read `4-deploy/CLAUDE.deploy.md`.

#### 2. Read Relevant Code

Read the files that are directly involved in the issue. Understand the current implementation before proposing changes. Follow the **read-before-write** rule — never modify code you haven't read.

If the user pointed to specific files, start there. Otherwise, use search tools (Grep, Glob) to locate the relevant code based on the problem description.

#### 3. Review Design Documents

Read relevant design documents in `2-design/` — at minimum the ones that cover the area being modified (architecture, data model, API design). Understand the intended design so you can assess whether the fix aligns with it or introduces a divergence.

#### 4. Check Applicable Decisions

For each affected component, review the `## Relevant Decisions` table in its `CLAUDE.component.md`:
- Identify any decisions whose trigger conditions match the current fix.
- Read the full decision file(s) for applicable decisions (follow the File column links).
- These decisions **must** be applied during implementation.

If the fix spans multiple components, review decisions from all involved components.

#### 5. Identify Related Requirements

If the fix relates to existing requirements (e.g., a bug where a requirement's acceptance criteria are not met), identify the relevant requirements from `1-spec/requirements/`. This helps ensure the fix actually addresses the root cause and that tests can verify the acceptance criteria.

If no existing requirement maps to the issue, that's fine — not every fix traces to a pre-existing requirement.

#### 6. Check for Constraint Tensions

After gathering all context (user description of the iussue, requirements, design documents, decisions, phase instructions), actively look for **tensions between authoritative sources**. A tension exists when two or more sources that the agent must follow pull in incompatible directions — satisfying one fully would require violating or bending another.

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

If no tensions are found, proceed to step 7.

#### 7. Evaluate Development Environment Needs

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

1. **Look for an existing environment convention** — check the decisions already identified for any that define environment setup conventions (e.g., a `DEC-python-venv` or `DEC-node-version-manager`). If no applicable decision exists, fall back to checking the component's `README`, `Makefile`, or equivalent files in the component directory for documented conventions. If a convention is found by either route (e.g., "use `.venv` in the component directory", "use `nvm` with `.nvmrc`"), follow it.

2. **If no convention exists and this task establishes one** — identify the ecosystem's standard practices for the tech stack across all relevant areas:
   - **Runtime isolation**: e.g., Python → virtual environment; Node.js → version manager + lockfile; Rust → `rustup` toolchain; Go → module-aware mode.
   - **Test infrastructure**: e.g., Python → pytest + configuration in `pyproject.toml`; Node.js → Vitest/Jest + config; Rust → built-in `cargo test`; Go → built-in `go test`.
   **Stop and present the choice to the user** before proceeding:
   - State which environment practices you intend to adopt and why they are standard for the stack.
   - Propose specifics (tool, location, configuration files).
   - Wait for confirmation.
   - After confirmation, record the convention as a decision (`DEC-*`) so future tasks follow it automatically (step 1 above).

3. **If an environment exists but the task modifies it** (e.g., adding dependencies, changing runtime version) — ensure commands run *within* the established environment (e.g., activate the venv before `pip install`, use the project's Node version before `npm install`).

#### 8. Confirm Approach

After gathering context, briefly describe your planned approach to the user:
- What files you will modify
- What the fix entails at a high level
- Any concerns or risks you've identified

Wait for confirmation before proceeding to implementation, **unless** the fix is straightforward and low-risk (e.g., a clear typo fix, an obvious off-by-one error). Use your judgment — when in doubt, ask to user.

### Implementation Log

Every fix execution must produce a durable log of the non-trivial operations and evaluations made during the run. The log is the paper trail a future reviewer or agent reads to understand what was done and why. The log is **append-only**: once the Understanding section has been populated and an entry has been written to the Execution log, they are never edited or removed — new entries are only appended.

#### Location and naming

- Logs live under `3-code/implementation-log/`, shared with the task logs produced by `SDLC-execute-task`. The `FIX-` prefix distinguishes fix logs from task logs.
- One file per fix. File name is the fix ID plus `.md` (e.g., `3-code/implementation-log/FIX-asset-list-pagination-off-by-one.md`).
- The template is `3-code/implementation-log/_fix_template.md`. Always start a new log file from this template — never from scratch, and never from the task log template (`_template.md`).

#### Log file setup

Before entering the Execution section, do the following in order:

1. **Check whether the log file exists** at `3-code/implementation-log/<FIX-ID>.md`.

2. **If the file does NOT exist (fresh fix):**
   - Copy `_fix_template.md` to `<FIX-ID>.md`.
   - Fill the header metadata: fix ID and today's date as `Started`. The header has no other mutable fields — the final outcome will be captured as a `[CONCLUSION]` entry appended at the end.

3. **If the file DOES exist (resumption of a prior fix):**
   - The user must already have confirmed this is a resumption during the "Assigning the fix identifier" subsection of Issue Elicitation. If the file exists but the user indicated this is a distinct fix that happened to share a slug, the slug should already have been disambiguated — no pre-existing log file should be in play. If one still is, **stop** and surface the inconsistency to the user before proceeding.
   - If the user confirmed resumption, the log should have already been read in full during Context Gathering's Resume branch (including its `## Understanding` section and its `[Q&A]`, `[RECONSIDER]`, and `[CONCLUSION]` entries). Append a `[RESUMED]` entry to the `## Execution log` section noting today's date and a brief indication of what triggered the resumption, then continue. Do not modify the pre-existing header, Understanding, or any earlier entry.

4. **Populate the Understanding section** (only on the fresh branch — skip this step when resuming a pre-existing log): fill the `## Understanding` section of the log with the elicited problem, scope boundaries, affected components and files, linked requirements (if any), relevant design artifacts, applicable decisions, and planned approach derived during Context Gathering. Include hyperlinks to every referenced artifact. Once filled, this section is immutable for the lifetime of the fix — if new understanding emerges later, record it as a `[RECONSIDER]` or `[NOTE]` entry in the Execution log instead.

Only after the log file is in place and the Understanding section has been populated may you proceed to the Execution section.

#### What to log during Execution

The goal is to preserve the **important operations** and the **important evaluations** the skill made, not to transcribe everything. Append entries to the `## Execution log` section as the work progresses — not only at the end — so that, if the run is interrupted, the log still reflects what happened up to that point. Never edit or delete an existing entry; if a previous entry turns out to be wrong or superseded, append a new `[RECONSIDER]` or `[NOTE]` entry that references and corrects it.

**Log:**

- **Every file write, create, or delete** as a `[WRITE]` entry — include the path and a one-line summary of what changed and why. Do not paste full diffs.
- **Every question asked to the user** as a `[Q&A]` entry — include both the question and the user's answer (verbatim or faithfully paraphrased).
- **Every reconsideration or mid-flight change of plan** as a `[RECONSIDER]` entry — what shifted and why.
- **Every problem detected** as a `[PROBLEM]` entry, and the corresponding intra-run correction as a `[FIX]` entry referencing it. (The `[FIX]` tag marks in-flight corrections during the skill run — it is distinct from the SDLC-fix skill itself, which is the outer context of the whole log.)
- **Every design gap** surfaced via the Design Gap Procedure as a `[DESIGN-GAP]` entry — include the resolution (design-document update, new decision, deviation accepted, etc.).
- **Every tension between authoritative sources** surfaced to the user as a `[TENSION]` entry — include the resolution.
- **Test-run outcomes worth preserving** (failures with root cause, debug iterations, the final green run) as `[TEST]` entries.
- **Any other important observation or evaluation** as a `[NOTE]` entry.

**Do not log:**

- Routine file reads (requirements, design docs, decisions — these are implicit context).
- Routine tool invocations that carry no decision weight.
- Verbatim long tool output — summarize instead.

#### Finalizing the log

When the skill finishes — whether the fix was completed or the run was stopped — append a `[CONCLUSION]` entry as the last entry of the Execution log. Its body must state the final outcome and summarize: files created/modified, tests added/updated, requirements and acceptance criteria satisfied (if any), new or updated decisions, design-document updates, task-plan impacts noted, and any pre-existing issues observed.

A log may accumulate multiple `[CONCLUSION]` entries across its lifetime: if a fix that already has a `[CONCLUSION]` is reopened (for instance, the user returns later to extend it or address a regression), the resumed run appends a fresh `[RESUMED]` entry and, when it finishes, a new `[CONCLUSION]` entry. Prior `[CONCLUSION]` entries are never edited or removed — the latest one reflects the current state, the earlier ones remain as historical record.

### Execution

Throughout the steps below, maintain the implementation log according to the rules in the "Implementation Log" section above — append entries as work progresses, not only at the end.

#### 1. Check for Design Gaps Before Coding

If the fix requires a significant divergence from the design documents, **stop and follow the Design Gap Procedure** (below) before writing implementation code.

#### 2. Implement

- Respect the component isolation rules in `3-code/CLAUDE.code.md`.
- Write clear, self-documenting code following language/framework conventions and established best practices (e.g., SOLID principles, DRY, separation of concerns, meaningful naming, proper error handling, security best practices).
- Prefer splitting code across multiple files over keeping a single large file, when compatible with language/framework conventions.
- Keep functions small and focused.
- Add comments only where logic isn't self-evident — do not add redundant or obvious comments.
- Use strict type checking where available.
- Apply all relevant decisions identified in Context Gathering step 4.
- **Minimize blast radius**: change only what is necessary to address the issue. Do not refactor surrounding code, add unrelated improvements, or "clean up" adjacent code unless the user explicitly asked for it.

#### 3. Write Tests

- **Bug fix tests**: when fixing a bug: (a) write a failing test that reproduces the bug before making any fix, (b) fix the bug and verify the test passes, (c) search the same component's codebase for the same bug pattern elsewhere — if found, **ask the user** whether to fix those occurrences too.
- **Change/enhancement tests**: when applying a modification or enhancement, write tests that verify the new or changed behavior.
- **Regression tests**: ensure existing tests still pass — the fix must not break other functionality.
- **Test organization**: follow the testing best practices and conventions of the project's tech stack (directory structure, naming, test runner configuration, fixtures, etc.).

#### 4. Run Tests and Verify

- Run the full test suite (or at minimum, all tests related to the current fix and its component).
- **All tests must pass** before the fix can be considered complete.
- If tests fail, analyze the failure, fix the issue, and re-run.
- If after **3 consecutive fix-and-rerun iterations** tests still fail, **stop** — explain to the user what is failing, what you have tried, and ask how they want to proceed (continue debugging, adjust the approach, simplify the tests).

#### 5. Check for Design Gaps After Coding

After implementing, evaluate whether any divergence from the design documents occurred. If so, follow the Design Gap Procedure.

#### 6. Evaluate New Decisions

After completing the fix, assess whether a new implementation pattern or convention emerged that should be documented:
- Did you make a non-obvious implementation choice that future tasks should replicate?
- Did you establish a pattern (error handling, UI interaction, data flow, naming) that should be consistent across the codebase?
- If yes: record a new decision.
- If no new decision is needed, skip this step.

#### 7. Do NOT Commit Automatically

Leave all changes for user review. Do not commit or push.

### Design Gap Procedure

A design gap is a divergence between **design documents** and what implementation requires. When updating design artifacts as part of this procedure, follow the procedures and instructions in `.claude/skills/SDLC-design/SKILL.md` — in particular the **Modifying Existing Design Documents** section (downstream effect checks, present changes, wait for confirmation), **Decision Triggers** (record new decisions when design choices are approved), **Current State Tracking** (update `CLAUDE.md`), and the cross-skill artifact procedures defined in `CLAUDE.md`.

**Minor divergence** (field renamed, type made more specific, optional field added): update the relevant `2-design/` file following the SDLC-design modification procedures, continue.

**Significant divergence** (new endpoint/entity, invalid architectural assumption, requirement that cannot be implemented as designed):

1. **Stop** — do not write implementation code.
2. **Surface** the gap: what the design says, what implementation needs, why they differ.
3. **Present options**: update design first, simplify to stay within design, or accept and record deviation as a decision.
4. **Wait for explicit approval** before proceeding.
5. **Act**: update `2-design/` or `1-spec/` as needed following the SDLC-design procedures (including downstream effect checks, decision recording, and Current State tracking), then implement.

### Interaction Style

- **Be conversational during elicitation** — the first part of this skill is a dialogue. Ask clear, focused questions. Don't overwhelm the user with all questions at once; adapt based on what they've already told you.
- After Context Gathering and before entering the Execution section, briefly state the elicited problem, the affected surface, the applicable decisions, and the planned approach. Do not ask for permission to begin unless genuine ambiguity remains (see Context Gathering step 8).
- **The implementation log is the gate into Execution, not a closing artifact.** After Context Gathering and before any Execution-path action — writing product files, running tests, editing design documents or `CLAUDE.md`, proposing new decisions — create `3-code/implementation-log/<FIX-ID>.md` from `_fix_template.md` and populate its `## Understanding` section in full, reusing the synthesis you just briefed. Then enter Execution, appending entries (`[WRITE]` / `[Q&A]` / `[RECONSIDER]` / `[PROBLEM]` / `[FIX]` / `[DESIGN-GAP]` / `[TENSION]` / `[TEST]` / `[NOTE]`) as work progresses — never batched at the end — and close with a `[CONCLUSION]`. Full rules live in the "Implementation Log" section above.
- During implementation, work autonomously — do not ask for confirmation at every step. The stop-and-ask points are explicitly defined in the instructions (elicitation, approach confirmation, design gaps, tension resolution, bug pattern occurrences).
- **After completing the fix, report the outcome and ask the user how they want to proceed** — e.g., review changes, commit, run additional tests.
- When a design gap is found, present it clearly with context, options, and trade-offs. Do not minimize the gap or push toward a specific resolution.

### Rules

- **Read-before-write**: always read existing files before proposing changes.
- **Elicit before implementing**: never start coding before understanding the problem. If the user's initial description is insufficient, ask for clarification.
- **Log is mandatory and append-only**: the per-fix implementation log under `3-code/implementation-log/<FIX-ID>.md` must exist and contain the Understanding section before the Execution section starts, and new entries (`[WRITE]` / `[Q&A]` / `[RECONSIDER]` / `[PROBLEM]` / `[FIX]` / `[DESIGN-GAP]` / `[TENSION]` / `[TEST]` / `[NOTE]`) must be appended as work progresses. Existing content is never edited or removed — corrections are appended as new entries. Each run of the skill terminates by appending a `[CONCLUSION]` entry; a reopened fix will therefore accumulate multiple `[RESUMED]` / `[CONCLUSION]` pairs over time.
- **Decisions are mandatory**: applicable decisions from the component's `CLAUDE.component.md` must be followed. If a decision conflicts with the fix, surface it to the user — do not silently ignore it.
- **Minimal changes**: change only what is necessary to address the reported issue. Do not add unrelated improvements or refactor surrounding code unless explicitly requested.
- **No auto-commit**: leave all changes for user review. Do not commit or push.
- **Traceability**: if the fix reveals that a requirement is missing or incorrect, surface it to the user and recommend `/SDLC-elicit` to address it. If the fix reveals that a planned task in `tasks.md` is affected (e.g., the fix pre-empts part of a future task, or a task's assumptions are now invalid), note it in the output.
- **Deploy awareness**: if the fix touches infrastructure as code (Terraform, Dockerfiles, CI/CD pipelines, deployment scripts, etc.), also read `4-deploy/CLAUDE.deploy.md`, review its decisions index, and follow its instructions alongside these.
- **External tool and library dependencies**: when a fix requires tools or libraries that are not available in the current environment (system packages like ffmpeg, database engines, runtime tools, etc.), **always notify the user before attempting to install them**. If the user approves and installation fails (e.g., no sudo access, missing package manager), provide the user with clear instructions on how to install the dependency themselves and **wait for the user to confirm** that installation is complete before proceeding. Never silently install system-level dependencies or work around their absence without user awareness.
- **Task plan impact**: if the fix changes behavior, interfaces, or data structures that planned tasks in `tasks.md` depend on, note which tasks may be affected in the output so the user can decide whether to update the implementation plan.

### Output

At the end, report:
- **Fix ID and log path** — the `FIX-<slug>` identifier assigned to this run and the path to its implementation log (`3-code/implementation-log/<FIX-ID>.md`)
- **Issue summary** — brief restatement of the problem that was fixed
- **What was done** — description of the changes made
- **Files modified** — list of files that were created or modified
- **Tests** — what tests were written or updated, and whether they pass
- **Decisions applied** — which decisions were followed during implementation (if any)
- **Constraint tensions** — whether any tension was found between authoritative sources and how it was resolved (if any)
- **Design gaps** — whether any design gap was found and how it was resolved (if any)
- **New decisions proposed** — whether a new decision was recorded (if any)
- **Task plan impact** — whether any planned tasks in `tasks.md` are affected by this fix (if applicable)
- **Pre-existing issues observed** — if during implementation or testing you encountered problems in code **outside** this fix's scope (e.g., type errors, linting warnings, test failures, deprecation warnings in pre-existing files), list them prominently under a dedicated heading. For each issue, state: (1) which file(s) and what the problem is, (2) why it was not fixed, and (3) a recommendation for when to address it.
