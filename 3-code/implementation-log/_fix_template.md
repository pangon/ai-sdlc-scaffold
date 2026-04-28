<!--
Template used by the SDLC-fix skill when it creates a new fix-log file. Copy this file
to `FIX-<kebab-slug>.md` in the same directory, then fill in the placeholders (wrapped
in `<...>`) as the skill progresses. Do not add this comment block to the copy.

This log is append-only: existing content is never rewritten. The Understanding section
is populated once, before the Execution section of the skill begins. Afterwards, only
new entries are appended to the Execution log.
-->

# <FIX-ID> — Fix Log

- **Fix:** <FIX-ID>
- **Started:** <YYYY-MM-DD>

---

## Understanding

_Populated once by the skill before it enters the "Execution" section. Captures the elicited problem, the affected surface, and the planned approach, with links to every referenced artifact. Not modified afterwards._

### Problem statement

<A concise description of the issue as elicited from the user — observed vs. expected behavior (for bugs), target outcome (for changes/enhancements), steps to reproduce and error signals if provided.>

### Scope boundaries

<What is explicitly in scope and what is out of scope. Any constraints on the approach stated by the user (e.g., "don't change the API contract", "must be backwards compatible").>

### Affected components and files

- [<component>](../<component>/) — <what is touched in this component>
- [<file>](../<component>/<path>) — <role of this file in the fix>

### Linked requirements

_Optional — include only if the fix relates to existing requirements (e.g., a bug where acceptance criteria are not met). Not every fix traces to a pre-existing requirement._

- [<REQ-ID>](../../1-spec/requirements/<REQ-ID>.md) — <one-line summary of how this requirement is involved>

### Relevant design artifacts

- [<file>](../../2-design/<file>.md) — <section / why it is relevant>

### Applicable decisions

- [<DEC-ID>](../../decisions/<DEC-ID>.md) — <how this decision constrains or shapes the implementation>

### Planned approach

<High-level description of the intended fix — what will change, in which file(s), and any risks identified during Context Gathering.>

---

## Execution log

_Append-only chronological narrative. New entries are appended as work progresses; existing entries are never edited or removed. Do not log routine file reads — focus on non-trivial operations and important evaluations._

Each entry starts with a date, a tag in square brackets, and a short title. Use one of the tags below:

- `[WRITE]` — file created, modified, or deleted. Include path and a one-line summary of the change and its purpose.
- `[Q&A]` — a question was asked to the user. Include the question and the user's answer (verbatim or faithfully paraphrased).
- `[RECONSIDER]` — a change of plan or approach mid-flight. Describe what shifted and why.
- `[PROBLEM]` — a problem was detected (test failure, unexpected state, broken assumption, etc.).
- `[FIX]` — an intra-run correction was applied to address a detected problem. Reference the `[PROBLEM]` entry it addresses. (This tag marks in-flight corrections during the skill run; it is distinct from the SDLC-fix skill itself, which is the outer context of the whole log.)
- `[DESIGN-GAP]` — a divergence between design documents and implementation needs was surfaced. Record the resolution (design-document update, new decision, etc.).
- `[TENSION]` — a tension between authoritative sources was surfaced to the user. Record the resolution.
- `[TEST]` — a test-run outcome worth capturing (failures, debug iterations, final green run).
- `[RESUMED]` — the skill resumed work on a pre-existing fix log.
- `[NOTE]` — any other important observation or evaluation made during execution.
- `[CONCLUSION]` — appended as the last entry when the skill finishes. The body must state the final outcome and summarize: files created/modified, tests added/updated, requirements/acceptance criteria satisfied (if any), new or updated decisions, design-document updates, task-plan impacts noted, and any pre-existing issues observed. A log may contain multiple `[CONCLUSION]` entries over time — if a fix is reopened after a prior `[CONCLUSION]`, a new `[RESUMED]` entry is appended and, when the fix is concluded again, another `[CONCLUSION]` is appended at the end.

### <YYYY-MM-DD> — [TAG] <short title>

<details>
