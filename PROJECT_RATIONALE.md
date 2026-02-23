# Project Rationale: ai-sdlc-scaffold

## What This Project Is

**ai-sdlc-scaffold** is a repository template for AI-first software development. It provides a folder structure, file templates, and AI agent instructions that encode a complete software development lifecycle (SDLC) where all project knowledge lives inside the repository itself.

It is not a tool, a library, or an application. It is a **starting scaffold** — you clone or copy it, then fill it in as your real project takes shape.

### Core Principles

1. **AI-first development model.** The scaffold is designed for a scenario where AI agents do the development work and a human supervises at a high level. The human defines objectives, reviews important decisions, and steers direction — but does not write code. Some structural choices are deliberately optimized for agent consumption over human ergonomics.

2. **Everything-in-repo.** Every artifact that explains the software — goals, requirements, assumptions, constraints, design decisions, task tracking — lives alongside the source code, version-controlled and always accessible. This gives agents the full context that human developers would normally carry in their heads or scattered across external tools.

3. **Context-window efficiency.** The scaffold is structured to minimize how many tokens an AI agent must load to do its job. This drives several design choices: hierarchical instruction files, separation of decision history from active records, phase-level indexes with trigger conditions, and hardcoded conventions in phase instructions (e.g. modular programming).

4. **Decision capture, not decision suppression.** AI agents are increasingly capable autonomous decision-makers. Rather than preventing them from deciding, the scaffold ensures decisions are *recorded in the repository* — not lost in reasoning tokens. This enables human review, consistency enforcement, and documentation. The system distinguishes decisions that require human approval from those an agent can auto-accept, and this boundary is expected to shift as agent capabilities advance.

## The Core Problem It Solves

In most software projects, the "why" behind the code is scattered across wikis, Confluence pages, Jira tickets, Slack threads, meeting notes, and people's heads. When an AI agent is given such a project, it sees only the code — it has no access to the context that produced it. This leads to:

- Agents making changes that contradict unrecorded architectural decisions
- Inability to trace why a feature exists or why it was built a certain way
- Repeated reasoning about decisions that were already made (wasting tokens and risking inconsistency)
- Important decisions buried in agent conversation logs, invisible to future sessions

## The Solution: Everything-in-Repo, AI-First

The scaffold makes the full project rationale — from business goals to deployment procedures — available as structured markdown files that agents can navigate efficiently.

**For AI agents**, the `CLAUDE.md` / `CLAUDE.<phase>.md` hierarchy provides layered instructions: global rules at the root, phase-specific guidance in each directory, and indexes that let agents decide which files to open and which to skip — all to keep context consumption minimal.

**For humans**, the same files create a self-documenting project where every decision is traceable from business goal to deployed code. The traceability chain is useful for human review even though the primary audience is agents.

## How It Is Organized

The project is divided into four sequential phases, each in its own directory:

| Phase | Purpose |
|---|---|
| **1-objectives** | Capture *what* to build and *why*: stakeholders, goals, user stories, requirements, assumptions, constraints |
| **2-design** | Define *how* to build it: architecture, data model, API design, and formal decision records |
| **3-code** | Build it: source code, tests, and a task tracker linked to requirements |
| **4-deploy** | Ship and operate it: infrastructure-as-code, deployment scripts, runbooks |

Each phase has:
- A `CLAUDE.<phase>.md` file with AI-specific instructions for that phase
- Templates (`_template.md`) for creating new artifacts
- Advisory phase gates defining minimum preconditions before advancing to the next phase

## Key Design Choices

### Context-Window Efficiency

Several structural choices exist specifically to reduce token consumption:

- **Hierarchical instructions** (`CLAUDE.md` → `CLAUDE.<phase>.md`): agents load only the global file and the relevant phase file, not all four phases at once.
- **Two-file decision records**: the active record (`DEC-NNN.md`) contains only what an agent needs during normal work; the history (`DEC-NNN.history.md`) is loaded only when evaluating or changing a decision.
- **Phase-level indexes with trigger conditions**: each `CLAUDE.<phase>.md` lists which decisions apply and when, so agents can skip irrelevant ones without opening them.
- **Hardcoded conventions over decision files**: routine patterns (like modular code structure) are written directly into phase instructions rather than spawning separate decision files.

### Two-File Decision Records

Every significant decision produces two files:
- `DEC-NNN.md` — the active record: what was decided and how to enforce it (optimized for AI agent consumption during normal work)
- `DEC-NNN.history.md` — the audit trail: what alternatives were considered, who decided, and a changelog (read only when evaluating or changing the decision)

### Human Involvement Taxonomy

Each decision records *how* it was made, on a spectrum from `human-decided` to `ai-proposed/auto-accepted`. This serves multiple purposes:
- Lets the human supervisor prioritize review effort on AI-originated decisions
- Tracks the evolving boundary between human and agent authority
- Enables future relaxation of review requirements as agent capabilities improve

### Traceability Chain

Artifacts link to each other explicitly:
```
Stakeholders → Goals → User Stories → Requirements → Decisions → Tasks → Code → Tests
```

Artifacts carry `Source` and/or `Source stakeholder` fields where applicable, linking each item back to its origin in the chain. Indexes in the phase instruction files provide navigable summaries.

### Graduated Safeguards

The scaffold defines three tiers of agent autonomy:

- **Always ask**: conflict resolution, design gaps, decision deprecation/supersession, phase gate advancement
- **Ask first time, then follow precedent**: naming conventions, error handling patterns, test structure
- **Decide and record**: routine implementation choices within established patterns

This is not a philosophical rejection of AI autonomy; it is a pragmatic safeguard for the current capability level. Agents are autonomous within their development tasks, but consequential project-level decisions are captured in the repository and flagged for review when appropriate. As agent reliability improves, these checkpoints can be relaxed.

### Phase Gates

Advisory checklists define minimum preconditions for advancing between phases (e.g., at least one goal and one requirement must be Approved before Design begins). Gates are not blocking — the agent warns the user if preconditions are unmet, but proceeds if the user confirms.

### Artifact Status Lifecycle

All artifacts with a Status field follow a defined lifecycle with explicit transition rules:
- **Draft → Approved**: only a human can approve
- **Approved → Implemented**: the agent marks this when all linked tasks reach Done
- **Any → Deprecated**: only a human can deprecate; the agent proposes and waits for approval

## What the Scaffold Provides Out of the Box

- Complete folder structure for all four phases
- File templates for every artifact type (goals, user stories, requirements, assumptions, constraints, decisions)
- One technology-agnostic example decision (error response format) to illustrate the pattern
- A task tracker linked to requirements
- A layered AI instruction system (`CLAUDE.md` → `CLAUDE.<phase>.md`)
- Decision management procedures (creation, deprecation, supersession)
- Phase gates and artifact status lifecycle
- Stakeholder definitions separated from agent instructions for clean editing
- Stub files for all referenced design documents (architecture, data model, API design)
- A `.gitignore` with technology-agnostic defaults
