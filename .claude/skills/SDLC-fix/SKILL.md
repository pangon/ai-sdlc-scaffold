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

Once you have sufficient context, summarize your understanding back to the user in a brief statement (e.g., "I understand the issue is X, affecting component Y, and the fix should Z. Proceeding.") and wait for confirmation before moving to Context Gathering.

### Context Gathering

Before writing any code, gather all necessary context:

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

### Execution

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
- During implementation, work autonomously — do not ask for confirmation at every step. The stop-and-ask points are explicitly defined in the instructions (elicitation, approach confirmation, design gaps, tension resolution, bug pattern occurrences).
- **After completing the fix, report the outcome and ask the user how they want to proceed** — e.g., review changes, commit, run additional tests.
- When a design gap is found, present it clearly with context, options, and trade-offs. Do not minimize the gap or push toward a specific resolution.

### Rules

- **Read-before-write**: always read existing files before proposing changes.
- **Elicit before implementing**: never start coding before understanding the problem. If the user's initial description is insufficient, ask for clarification.
- **Decisions are mandatory**: applicable decisions from the component's `CLAUDE.component.md` must be followed. If a decision conflicts with the fix, surface it to the user — do not silently ignore it.
- **Minimal changes**: change only what is necessary to address the reported issue. Do not add unrelated improvements or refactor surrounding code unless explicitly requested.
- **No auto-commit**: leave all changes for user review. Do not commit or push.
- **Traceability**: if the fix reveals that a requirement is missing or incorrect, surface it to the user and recommend `/SDLC-elicit` to address it. If the fix reveals that a planned task in `tasks.md` is affected (e.g., the fix pre-empts part of a future task, or a task's assumptions are now invalid), note it in the output.
- **Deploy awareness**: if the fix touches infrastructure as code (Terraform, Dockerfiles, CI/CD pipelines, deployment scripts, etc.), also read `4-deploy/CLAUDE.deploy.md`, review its decisions index, and follow its instructions alongside these.
- **External tool and library dependencies**: when a fix requires tools or libraries that are not available in the current environment (system packages like ffmpeg, database engines, runtime tools, etc.), **always notify the user before attempting to install them**. If the user approves and installation fails (e.g., no sudo access, missing package manager), provide the user with clear instructions on how to install the dependency themselves and **wait for the user to confirm** that installation is complete before proceeding. Never silently install system-level dependencies or work around their absence without user awareness.
- **Task plan impact**: if the fix changes behavior, interfaces, or data structures that planned tasks in `tasks.md` depend on, note which tasks may be affected in the output so the user can decide whether to update the implementation plan.

### Output

At the end, report:
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
