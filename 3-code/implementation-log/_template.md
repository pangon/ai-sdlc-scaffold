<!--
Template used by the SDLC-execute-task skill when it creates a new implementation-log
file. Copy this file to `<TASK-ID>.md` in the same directory, then fill in the placeholders
(wrapped in `<...>`) as the skill progresses. Do not add this comment block to the copy.

This log is append-only: existing content is never rewritten. The Understanding section
is populated once, before the Execution section of the skill begins. Afterwards, only
new entries are appended to the Execution log.
-->

# <TASK-ID> — Implementation Log

- **Task:** [<TASK-ID>](../tasks.md)
- **Started:** <YYYY-MM-DD>

---

## Understanding

_Populated once by the skill before it enters the "Execution" section. Captures the synthesized scope derived from the authoritative sources and links to every referenced artifact. Not modified afterwards._

### Synthesized scope

<A concise description of what this task must actually deliver, derived from the authoritative sources. If the synthesized scope diverges from the brief description in `tasks.md`, note the divergence and why.>

### Linked requirements

- [<REQ-ID>](../../1-spec/requirements/<REQ-ID>.md) — <one-line summary of what this requirement contributes and which acceptance criteria this task is expected to satisfy>

### Relevant design artifacts

- [<file>](../../2-design/<file>.md) — <section / why it is relevant>

### Applicable decisions

- [<DEC-ID>](../../decisions/<DEC-ID>.md) — <how this decision constrains or shapes the implementation>

### Downstream tasks that depend on this

- <TASK-ID> — <what the downstream task expects from this task's output>

---

## Execution log

_Append-only chronological narrative. New entries are appended as work progresses; existing entries are never edited or removed. Do not log routine file reads — focus on non-trivial operations and important evaluations._

Each entry starts with a date, a tag in square brackets, and a short title. Use one of the tags below:

- `[WRITE]` — file created, modified, or deleted. Include path and a one-line summary of the change and its purpose.
- `[Q&A]` — a question was asked to the user. Include the question and the user's answer (verbatim or faithfully paraphrased).
- `[RECONSIDER]` — a change of plan or approach mid-flight. Describe what shifted and why.
- `[PROBLEM]` — a problem was detected (test failure, unexpected state, broken assumption, etc.).
- `[FIX]` — a fix was applied. Reference the `[PROBLEM]` entry it addresses.
- `[DESIGN-GAP]` — a divergence between design documents and implementation needs was surfaced. Record the resolution (design-document update, new decision, etc.).
- `[TENSION]` — a tension between authoritative sources was surfaced to the user. Record the resolution.
- `[TEST]` — a test-run outcome worth capturing (failures, debug iterations, final green run).
- `[RESUMED]` — the skill resumed work on a pre-existing `In Progress` task.
- `[NOTE]` — any other important observation or evaluation made during execution.
- `[CONCLUSION]` — appended as the last entry when the skill finishes (Done, Decomposed, Blocked, or Cancelled). The body must state the final status and summarize: files created/modified, tests added/updated, requirements/acceptance criteria satisfied, new or updated decisions, design-document updates, and any pre-existing issues observed. A log may contain multiple `[CONCLUSION]` entries over time — if a task is reopened after a prior `[CONCLUSION]`, a new `[RESUMED]` entry is appended and, when the task is concluded again, another `[CONCLUSION]` is appended at the end.

### <YYYY-MM-DD> — [TAG] <short title>

<details>
