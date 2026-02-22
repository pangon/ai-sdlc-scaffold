# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with this repository.

## Language Policy

**All AI outputs must be in English**, regardless of the language used in user prompts. This applies to:
- Code and inline comments
- Documentation (markdown files, README, etc.)
- Configuration files and their comments
- Commit messages
- Response text and explanations

The user may write prompts in any language, but Claude must always respond and produce artifacts in English.

---

## Project Overview

<!-- TODO: Describe your project here -->

This repository is a **scaffold template** for projects that want to adopt a structured, AI-assisted software development lifecycle. Clone or copy it to bootstrap any new project with the same approach.

The central paradigm is **everything-in-repo**: objectives, requirements, architecture, decisions, and development task tracking all live alongside the source code — versioned, searchable, and always in sync with the implementation. This makes the full history of *why* the software is built the way it is available to both humans and AI assistants at all times.

---

## Repository Structure

The repository is organized into development phases. Claude should respect this organization and guide users to the appropriate section.

```
project/
├── CLAUDE.md                 # Main AI instructions (this file)
├── README.md                 # Project overview and quick start
│
├── 1-objectives/            # High-level goals and requirements
│   ├── CLAUDE.objectives.md  # Phase-specific AI instructions
│   ├── goals.md              # Project goals and success criteria
│   ├── requirements.md       # Functional and non-functional requirements
│   └── constraints.md        # Technical and business constraints
│
├── 2-design/                # Architecture and design decisions
│   ├── CLAUDE.design.md      # Phase-specific AI instructions
│   ├── architecture.md       # System architecture overview
│   ├── data-model.md         # Data structures and schemas
│   ├── api-design.md         # API specifications
│   ├── decisions.md          # Decision format, templates, and agent navigation rules
│   └── decisions/            # Decision Records (DEC-NNN)
│
├── 3-code/                  # Implementation
│   ├── CLAUDE.code.md        # Phase-specific AI instructions
│   ├── tasks.md              # Development task tracker
│   └── <codebase>/           # One or more named codebases (e.g. frontend, cli, backend)
│       ├── src/              # Source code for this codebase
│       └── tests/            # Test suites for this codebase
│
└── 4-deploy/                # Deployment and operations
    ├── CLAUDE.deploy.md      # Phase-specific AI instructions
    ├── infrastructure/       # IaC configurations
    ├── scripts/              # Deployment scripts
    └── runbooks/             # Operational procedures
```

---

## Phase-Specific Instructions

Each phase directory contains a `CLAUDE.<phase>.md` file with detailed instructions for that phase. Claude should:

1. **Check for phase-specific instructions** when working in a phase directory
2. **Follow the hierarchy**: phase-specific instructions extend (not override) this file

### Phase Workflow

| Phase | Directory | Focus |
|-------|-----------|-------|
| **Objectives** | `1-objectives/` | Define what to build and why |
| **Design** | `2-design/` | Define how to build it |
| **Code** | `3-code/` | Build it |
| **Deploy** | `4-deploy/` | Ship and operate it |

Claude should help users navigate between phases and ensure work is placed in the correct location.

---

## AI Collaboration Guidelines

### When Starting Work

1. Identify which phase the task belongs to
2. Check for phase-specific `CLAUDE.<phase>.md` instructions
3. Create necessary directories and files if they don't exist
4. Work within the appropriate phase structure

### During Development

- Update relevant phase documents when making changes
- Cross-reference between phases when appropriate (e.g., link design docs from code)
- **Ask before deciding**: When you spot a related issue, potential improvement, or ambiguous situation during a task, **ask the user explicitly** instead of silently deciding to act or not act. Never assume the user's intent — always surface the decision. Example: "I noticed the same problem exists in file X — should I fix it too?"

### After Making Changes

Claude should evaluate whether to:

1. **Update this file** (`CLAUDE.md`) if:
   - New project-wide patterns emerge
   - Build commands or architecture change significantly
   - New tools or workflows are introduced

2. **Update phase-specific files** (`CLAUDE.<phase>.md`) if:
   - Phase-specific patterns or conventions are established
   - New tools specific to that phase are added
   - Common tasks in that phase should be documented

3. **Create new instruction files** if:
   - A subtask becomes complex enough to warrant dedicated guidance
   - A new phase or sub-phase emerges
   - A workflow becomes complex enough to need documentation

Claude should proactively suggest these updates when relevant.

---

## Decisions

All project decisions — architecture choices, design conventions, coding patterns, process rules — live in a single location: `2-design/decisions/`.

Each decision consists of two files linked by naming convention:

- **`DEC-NNN-short-name.md`** — the active record: context, decision, and enforcement rules written for AI agents. Read this during normal task execution.
- **`DEC-NNN-short-name.history.md`** — the trail: alternatives considered, reasoning, human involvement classification, and changelog. Read this only when evaluating a decision or proposing a change.

Phase-specific decision indexes (with trigger conditions) are maintained in `2-design/CLAUDE.design.md` and `3-code/CLAUDE.code.md`. A decision may appear in both if it affects both phases — with phase-specific phrasing for trigger conditions.

For the full format specification, templates, and agent navigation rules, see [`2-design/decisions.md`](2-design/decisions.md).

---

## Continuous Improvement

This file and all `CLAUDE.*.md` files are living documents. After significant work sessions, Claude should:

1. Review if current instructions are still accurate
2. Identify patterns that could be documented
3. Suggest improvements or new instruction files
4. Keep instructions concise and actionable
