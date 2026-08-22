# Shared Procedure: Development Environment Evaluation

_Included by `SDLC-execute-task` and `SDLC-fix`. Read when the including skill directs here._

Assess whether the work requires **development environment setup or interaction** — runtime isolation, version management, package installation, build tooling, test infrastructure, or similar concerns.

**When this evaluation applies:**

- The work creates or modifies project scaffolding (package manifests, lockfiles, build configuration)
- The work installs, upgrades, or removes dependencies
- The work sets up or configures test infrastructure (test runners, coverage tools, assertion libraries, fixtures)
- The work runs build commands, test runners, linters, or other tooling
- The component's tech stack has well-known environment isolation conventions (e.g., Python virtual environments, Node version managers, Ruby gemsets, Rust toolchains)

**When it does NOT apply:**

- The work only edits source code without running any commands
- The environment is already established and documented (e.g., a prior task created and documented the venv, and its activation path is recorded in a decision or component instructions)

**If it applies, check in order:**

1. **Look for an existing environment convention** — check the applicable decisions already identified during preparation for any that define environment setup conventions (e.g., a `DEC-python-venv` or `DEC-node-version-manager`). If no applicable decision exists, fall back to checking the component's `README`, `Makefile`, or equivalent files in the component directory for documented conventions. If a convention is found by either route (e.g., "use `.venv` in the component directory", "use `nvm` with `.nvmrc`"), follow it.

2. **If no convention exists and this work establishes one** — identify the ecosystem's standard practices for the tech stack across all relevant areas:
   - **Runtime isolation**: e.g., Python → virtual environment; Node.js → version manager + lockfile; Rust → `rustup` toolchain; Go → module-aware mode.
   - **Test infrastructure**: e.g., Python → pytest + configuration in `pyproject.toml`; Node.js → Vitest/Jest + config; Rust → built-in `cargo test`; Go → built-in `go test`.

   **Stop and present the choice to the user** before proceeding:
   - State which environment practices you intend to adopt and why they are standard for the stack.
   - Propose specifics (tool, location, configuration files).
   - Wait for confirmation.
   - After confirmation, record the convention as a decision (`DEC-*`) so future work follows it automatically (step 1 above).

3. **If an environment exists but the work modifies it** (e.g., adding dependencies, changing runtime version) — ensure commands run *within* the established environment (e.g., activate the venv before `pip install`, use the project's Node version before `npm install`).
