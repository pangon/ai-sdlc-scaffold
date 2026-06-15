# Post-finalize follow-up cycles

Some pre-existing issues surfaced by the developer (in `[Pre-existing]` implementation-log entries) or the reviewer (in their findings) can be cleanly fixed in a small, mechanical follow-up PR. After the main task PR is merged (Finalize step 5), walk those issues, pick the auto-fixable ones, and ship **one follow-up PR per issue** before tearing down the team.

The agent team is **kept alive** through these cycles — reuse the same `developer` and `reviewer` via `SendMessage` so they carry the run's accumulated context. Do **not** spawn fresh Agents for follow-ups.

Each follow-up cycle owns a **dedicated worktree** at `.claude/worktrees/<cycle-timestamp>/` (a distinct UTC start timestamp) and a **dedicated orchestration log file** at `<cycle-worktree>/orchestration-log/<cycle-timestamp>.md`. Copy from `<cycle-worktree>/orchestration-log/_followup-template.md` (not `_template.md`); the header rules and tag-set delta are in `## Follow-up log format` below. The main run's worktree is not reused (it is removed at Finalize step 8 after all cycles complete) and the main log stays closed — nothing is appended to it during follow-up cycles.

## Follow-up log format

The follow-up log follows the same format rules, tag definitions, and append-only discipline as the main orchestration log, with these differences:

- The header is populated once at file creation, with no `<TBD>` phase — the cycle's slug and branch are already known when the log is opened. The body opens with `## Originating pre-existing issue` (the issue being fixed) in lieu of the main log's `## User prompt`.
- `[TEAM]` is never used — agent-team lifecycle is run-scoped and already recorded in the main log.
- `[BLOCKED]` is never used — a follow-up cycle's failure path is an abort (a `[CONCLUSION]` with `aborted: <reason>` status), never a run-level halt (see Abort conditions).
- `[CONCLUSION]` additionally states the cycle outcome: `merged-ready` or `aborted: <reason>`.

## Triage

For each pre-existing issue surfaced during the run, classify as one of:

- **Auto-fixable** — concrete, small, does not change spec or design, no conflict with other in-flight work. Examples: a missing row in `3-code/tasks.md`, a stale link in a `CLAUDE.*.md`, a missing artifact index entry, a dead variable, a typo, a missing decision record, a small refactor that doesn't change spec or design.
- **Not auto-fixable** — large, vague, changes spec or design, or conflicts with other in-flight work.

When in doubt, classify as **not auto-fixable** — a missed auto-fix is just a follow-up for the user; a wrong auto-fix lands incorrect work on `main`.

## Cycle (sequential, one per auto-fixable issue — never in parallel)

1. **Create a fresh worktree for this cycle, off the just-merged `main`.** From the orchestrator's main checkout:
   - `git fetch origin main` so the worktree's base is the merged tip.
   - Capture `git rev-parse origin/main` as the **cycle's base SHA** — passed to the developer (step 3) and the reviewer (step 5).
   - Pick `<kebab-slug>` — a few-word summary of the fix (e.g., `add-task-portal-csp-headers`, `fix-stale-link-claude-code`). If `claude/followup/<kebab-slug>` collides locally or remotely, append a progressive suffix as in the main-task branch-naming rule.
   - Pick the cycle's UTC timestamp `<cycle-timestamp>` — it names both the cycle's worktree directory and its orchestration log file.
   - `git worktree add -b claude/followup/<kebab-slug> .claude/worktrees/<cycle-timestamp> origin/main`.

2. **Open the follow-up orchestration log file inside the cycle's worktree.** Copy `<cycle-worktree>/orchestration-log/_followup-template.md` to `<cycle-worktree>/orchestration-log/<cycle-timestamp>.md`. Populate the header. Replace the `## Originating pre-existing issue` placeholder with a faithful quote of the pre-existing-issue paragraph as surfaced by the developer or reviewer.

3. **Delegate via `SendMessage`** to the existing `developer`. Instruct it to run `/SDLC-fix` with:
   - A concise description of the pre-existing issue, paraphrased faithfully from the developer's `[Pre-existing]` entry or the reviewer's finding.
   - The cycle's worktree absolute path, the branch name `claude/followup/<kebab-slug>`, and the cycle's base SHA.
   - The expected scope and an instruction to **abort and report back** if the fix exceeds it.
   - The same pre-write checkpoint discipline as for the main task: synthesis before any write, your go-ahead, then proceed. (No branch rename happens at this checkpoint — the branch name is already final.)
   Append a `[DELEGATED]` entry to the follow-up log.

4. **Q&A as needed** — same protocol as for the main task. Append `[Q-DEV]` / `[A-DEV]` entries to the follow-up log as they happen.

5. **Reviewer iteration** with the existing `reviewer` via `SendMessage`, pointing to the cycle's worktree path, the cycle's base SHA, the new diff, and the iteration number (cycles restart at 1). **Cap at 3 iterations** — if not approved after the third, abort the cycle. Append `[REVIEW]` / `[FINDINGS]` / `[RELAY]` entries to the follow-up log.

6. **Commit, push, open PR, merge** — same discipline as Finalize steps 1–5 for the main task, all scoped to the cycle's worktree:
   - **Append `[CONCLUSION]` to the follow-up log first**. State branch, follow-up review iteration count, a short summary of the fix, the outcome (`merged-ready`), and any incidental observations.
   - **Stage and commit, inside the cycle's worktree.** Single commit, message following the project's `git log` style. Append a `[GIT]` entry to the follow-up log **before** the commit (so it ships alongside `[CONCLUSION]`).
   - **Push** the branch from the worktree: `git -C <cycle-worktree> push -u origin claude/followup/<kebab-slug>`.
   - **Open the PR** via `gh pr create` from inside the cycle's worktree. Title: `[Follow-up] <one-line scope>`. PR body cites the originating task ID, the main task's PR/merge SHA, the main orchestration log filename, the follow-up orchestration log filename, and quotes the pre-existing-issue paragraph as surfaced. Summarize the fix.
   - **Merge** with the same strategy as the main task (`--merge` / `--squash` / `--rebase`), applying the same checks-wait discipline as Finalize step 5 (pending required checks are watched via `gh pr checks --watch`, not treated as failures; a genuine check failure or the wait cap aborts the cycle).

7. **Remove the cycle's worktree and local branch.** Once the cycle's PR is merged:
   - `git worktree remove .claude/worktrees/<cycle-timestamp>`
   - `git branch -d claude/followup/<kebab-slug>` (or `-D` if `-d` refuses — anomaly worth noting)
   Failures here are cosmetic — do not halt or abort the cycle.

## Abort conditions

A follow-up cycle aborts when any of the following occur:

- The developer reports back saying the scope is bigger than expected, the fix would change spec/design, or the issue is not fixable in one iteration.
- The reviewer blocks (cannot complete the review, or finds the fix unsalvageable).
- The follow-up review iteration cap is hit without approval.
- `gh pr merge` is rejected by branch protection, required checks, or any other platform gate.
- A required tool install hits an elevation requirement the developer cannot satisfy — abort with `aborted: install elevation required, manual steps documented in the [ENV-INSTALL] entry`; the final `[ENV-INSTALL]` entry must enumerate the exact root-level commands an operator with `sudo` would need (the abort path's commit + push preserves them on the branch).
- A question arises that only the user can answer while running headless — append `[Q-USER]` recording the question, then abort.
- Any other operational error (push fails, PR creation fails, network error, etc.).

On abort:

1. **Close the follow-up log file** by appending a final `[CONCLUSION]` with `aborted: <reason>` status (state branch, follow-up review iteration count, what triggered the abort, and the reason). Skip if the follow-up log file was never created (abort happened during cycle step 1).
2. **Commit the abort state from inside the cycle's worktree** so the trail survives on the branch. Single commit indicating the abort (message: `WIP — follow-up abort: <one-line reason>`), staging everything still uncommitted — the follow-up log file, any other orchestrator-side state, and the developer's files alike (preservation beats commit hygiene; the WIP marker makes clear this is not mergeable work). Append a `[GIT]` entry to the follow-up log *before* this commit so the commit captures it.
3. **Push the branch** so the work and the trail are preserved on the remote for the user to pick up: `git -C <cycle-worktree> push -u origin claude/followup/<kebab-slug>`.
4. If a PR was opened before the abort, leave it open — the abort commit will appear on the PR. Add a brief PR comment (`gh pr comment`) noting the abort and pointing to the follow-up log file on the branch.
5. **Leave the cycle's worktree on the filesystem** at `.claude/worktrees/<cycle-timestamp>/` for the user to inspect.
6. **Do not halt the run.** Follow-ups are best-effort; the main task already shipped successfully. Move on to the next auto-fixable issue, or to Finalize step 7 (team teardown) if no more remain.

A follow-up abort never becomes a `[BLOCKED]` orchestration-log entry and never opens a halt-termination issue. The run's success classification was set when the main PR merged at Finalize step 5.

## Limits

- **At most 5 follow-up cycles per run.** If more than 5 issues qualify, take the 5 highest-confidence; list the rest under `Pre-existing issues observed (not auto-fixed)` with a `(follow-up deferred — per-run cap)` annotation.
- **Strictly sequential.** A cycle must finish before the next starts. No parallel branches, no overlapping reviews.
- **Originating-run scope only.** Follow-up cycles fix issues *this run* surfaced. Do not scan `main`, re-open prior runs' termination issues, or chain a follow-up onto another follow-up's findings.
