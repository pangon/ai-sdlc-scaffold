# Shared Rules: Implementation Log

_Included by `SDLC-execute-task` (task logs) and `SDLC-fix` (fix logs). Read when the including skill directs here. Location, naming, template, and log-file setup are defined by each including skill; the rules below govern the log's content and lifecycle._

Every run must produce a durable log of the non-trivial operations and evaluations made during it. The log is the paper trail a future reviewer or agent reads to understand what was done and why. The log is **append-only**: once the Understanding section has been populated and an entry has been written to the Execution log, they are never edited or removed — new entries are only appended.

## What to log during Execution

The goal is to preserve the **important operations** and the **important evaluations** the skill made, not to transcribe everything. Append entries to the `## Execution log` section as the work progresses — not only at the end — so that, if the run is interrupted, the log still reflects what happened up to that point. Never edit or delete an existing entry; if a previous entry turns out to be wrong or superseded, append a new `[RECONSIDER]` or `[NOTE]` entry that references and corrects it.

**Log:**

- **Every file write, create, or delete** as a `[WRITE]` entry — include the path and a one-line summary of what changed and why. Do not paste full diffs.
- **Every question asked to the user** as a `[Q&A]` entry — include both the question and the user's answer (verbatim or faithfully paraphrased).
- **Every reconsideration or mid-flight change of plan** as a `[RECONSIDER]` entry — what shifted and why.
- **Every problem detected** as a `[PROBLEM]` entry, and the corresponding fix as a `[FIX]` entry referencing it. (In fix logs, `[FIX]` marks in-flight corrections during the run — distinct from the SDLC-fix skill itself, which is the outer context of the whole log.)
- **Every design gap** surfaced via the Design Gap Procedure as a `[DESIGN-GAP]` entry — include the resolution (design-document update, new decision, deviation accepted, etc.).
- **Every tension between authoritative sources** surfaced to the user as a `[TENSION]` entry — include the resolution.
- **Test-run outcomes worth preserving** (failures with root cause, debug iterations, the final green run) as `[TEST]` entries.
- **Any other important observation or evaluation** as a `[NOTE]` entry.

**Do not log:**

- Routine file reads (requirements, design docs, decisions — these are implicit context).
- Routine tool invocations that carry no decision weight.
- Verbatim long tool output — summarize instead.

## Finalizing the log

When the run finishes — regardless of outcome (completed, decomposed, blocked, cancelled, or stopped) — append a `[CONCLUSION]` entry as the last entry of the Execution log. Its body must state the final status or outcome and summarize: files created/modified, tests added/updated, requirements and acceptance criteria satisfied (if any), new or updated decisions, design-document updates, task-plan impacts (if any), and any pre-existing issues observed.

A log may accumulate multiple `[CONCLUSION]` entries across its lifetime: if a run that already has a `[CONCLUSION]` is reopened, the resumed run appends a fresh `[RESUMED]` entry and, when it finishes, a new `[CONCLUSION]` entry. Prior `[CONCLUSION]` entries are never edited or removed — the latest one reflects the current state, the earlier ones remain as historical record.
