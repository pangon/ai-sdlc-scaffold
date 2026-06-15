# Developer-environment dependency installation

A recurring developer Q&A pattern is "tool X is not installed on this host — how should I proceed?" (typical examples: language runtime/SDK, container runtime, vendor CLI, language package manager, common utilities). The orchestrator must instruct the developer neither to hand-author code without ever running the tool, nor to stash the tool into an undocumented private location the project can't see.

## When you can authorize the install

Authorize the developer to install the tool when **all four** criteria hold:

- **Known** — widely recognized in its ecosystem (not obscure, not single-author, not experimental).
- **Common** — the standard or de-facto-standard tool for the use case named by the task or the design/decision artifacts.
- **Safe** — distributed via the tool's official channel (vendor site, official OS package repository, vendor-signed installer). Authorize only the official path; never authorize installs from a random third-party mirror or a curl-piped script hosted on a personal site.
- **Essential** — the task or its acceptance criteria genuinely require it.

If any criterion fails, do not authorize the install — handle via the standard Q&A loop (escalate to the user with `[Q-USER]`, or redirect the developer to a different approach).

## How to install: pick the tool's own best practice for the use case

Pick the install method matching the tool's **production-grade best practice for the project's use case**. Two patterns are routinely correct — treat them as peers:

1. **System-wide install via the OS package manager or vendor's globally-rooted installer.** The standard for tools without a project-scoped version model — `dotnet` via Microsoft's APT feed, `docker`, vendor CLIs (`gh`, `kubectl`, `terraform`, `aws`), utilities (`jq`, `yq`). The tool ends up on the default `PATH` of any future shell, so project scripts, `Dockerfile` builds, deploy runbooks, and CI/CD just work.
2. **Per-project version-pinned install via the tool's native version-manager**, when that *is* the tool's own best practice for project-level reproducibility. Standard examples: Node.js via `nvm` with `.nvmrc`, Python via a project-local `venv` with a pinned `pyproject.toml` / `requirements.txt`, Python via `pyenv` with `.python-version`, `asdf` with `.tool-versions`, `mise` with its config, Ruby via `rbenv` / `.ruby-version`. The version-pinning file lives in the component and ships with the PR, so a fresh shell that runs the documented activation step from the component root gets the same version every time. The reproducibility guarantee matches a system-wide install; the scope is just narrower.

**Whichever pattern you pick, two things must hold for the install to be acceptable**:

- **A decision file records the chosen pattern** at the appropriate scope (component-scoped `DEC-*` for single-component tools; system-wide `DEC-*` for tools used by multiple components). Without it, future sessions have no breadcrumb on which install path to honor and are likely to drift.
- **The affected component's `CLAUDE.component.md` (and its deploy runbook / `README` if any) reference the decision** so a fresh contributor — AI or human — reading the docs from a clean checkout sees exactly how to install and invoke the tool. For pattern (2), the version-pinning file (`.nvmrc`, `.python-version`, `.tool-versions`, project `venv` layout, etc.) must be **committed to the component** so the pin ships with the PR.

**What's NOT acceptable** is an ad-hoc install matching neither pattern and lacking decision or component-doc footprint. Example to avoid: `~/.dotnet/` via `dotnet-install.sh` — dotnet's best practice is system-wide via Microsoft's APT feed, and the per-user install was undocumented and invisible to anyone re-provisioning the host.

## Deciding between the two patterns

- If a `DEC-*` already pins the install pattern for the *same* tool, follow it.
- If a `DEC-*` exists for an *analogous* tool, follow the precedent unless ecosystems differ enough to justify a different call.
- If this is the first time the tool comes up in the project, pick the more idiomatic pattern for the ecosystem (e.g., `nvm` + `.nvmrc` for Node, system-wide for `dotnet`, project-local `venv` for Python, system-wide for `docker` / vendor CLIs / utilities) and record the new decision before authorizing.
- If both patterns are legitimate and project precedent + ecosystem idiom don't give a clear answer, escalate to the user with `[Q-USER]`.

## How to direct the developer

When you authorize an install, instruct the developer to:

1. Use the chosen pattern's **official commands** for the host's OS/distribution. Name the exact commands (system-wide example: `sudo apt-get update && sudo apt-get install -y dotnet-sdk-10.0` after configuring Microsoft's package repository; per-project example: install `nvm` via its official script, then `nvm install` from the component root with `.nvmrc` present).
2. **For pattern (2)**, commit the version-pinning file (`.nvmrc`, `.python-version`, `.tool-versions`, etc.) to the component. **For pattern (1)**, the deploy runbook / `CLAUDE.component.md` records the install command + version target — there is no per-project pin file, the decision *is* the pin.
3. **Verify the install** with the canonical version check (`dotnet --list-sdks`, `node --version`, `terraform version`, etc.). For pattern (1), the check runs from a fresh shell with no session-local `PATH` prefix; for pattern (2), from the component root after the documented activation step (`nvm use`, `source .venv/bin/activate`, `pyenv shell`, `mise activate`, etc.) — that must be recorded in `CLAUDE.component.md`.
4. **Record the install and its decision in the implementation log** (tool, version, install command, source/repo, the new `DEC-*` filename). Update the affected component's `CLAUDE.component.md` to cite the new decision, and update the deploy runbook (if any) to reflect the install method.

Log every install attempt and its outcome via `[ENV-INSTALL]` in the orchestration log.

## When the install requires elevation the developer cannot obtain

If you chose a **system-wide install pattern** and the developer reports it requires `root`/`sudo` but lacks password-less `sudo` (or equivalent elevation), **do not attempt to acquire root yourself, and do not direct the developer to**. Specifically, do not:

- Run `sudo …` from the orchestrator's shell (cannot supply an interactive password).
- Escalate via `AskUserQuestion` asking for a password or interactive `sudo` access — passwords are never collected through this channel.
- **Re-decide to switch to a per-project version-manager pattern just to dodge `sudo`.** The version-manager pattern is a first-choice option when it *is* the tool's best practice — not a fallback after the chosen pattern fails. If the up-front pattern choice was wrong on its merits, escalate to the user for a project-level re-decision.
- Direct the developer to a third-party binary archive or curl-piped vendor script as a "no-root" workaround for a system-wide-pattern tool.

Instead, **halt the run via the standard halt path**:

1. Append a final `[ENV-INSTALL]` entry recording the elevation requirement and enumerating the exact root-level commands an operator with `sudo` would need to run.
2. Append a `[BLOCKED]` entry citing the install-elevation requirement as the halt reason. Reference the `[ENV-INSTALL]` entry above for the exact manual steps.
3. Follow the halt-termination protocol. The halt issue's `Development environment installations` section restates the manual install steps so the user doesn't have to open the orchestration log.

(Pattern (2) installs typically do not run into this halt path — version-managers like `nvm`, `pyenv`, `asdf`, project-local `venv` are designed for unprivileged user installs. If a pattern-(2) install nonetheless hits an elevation requirement the developer cannot satisfy, the same halt protocol applies.)
