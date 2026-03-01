---
name: SDLC-init
description: Guided project initialization for the SDLC scaffold. Use when setting up a new project or re-configuring an existing one. Sets the project description in CLAUDE.md so AI agents have the right context from the start.
---

## Instructions

You are guiding the user through the initial setup of a new project based on the AI SDLC Scaffold, interactively configuring the project description and metadata.

### Workflow

1. **Read** the current `## Project Overview` section in `CLAUDE.md`. If it already contains a real description (not just placeholder text), show it to the user and ask whether they want to replace or refine it.
2. **Ask** the user to describe their project: what it does and what problem it solves. As an alternative, offer to auto-generate a summary from the repository name. Do not ask for technology stack or architectural details — those belong in later phases.
3. **Write** a concise project description optimized for AI agent consumption. Replace any placeholder comments and generic scaffold text.
4. **Update `### Current State`** if present — set it to reflect the actual project status (e.g., "The project has been initialized. The next step is to populate the Objectives phase.").
5. **Confirm** — show the user the final text that was written.

### Rules

- If the user wants to skip, leave the section unchanged.
- Write content optimized for AI agent consumption: clear, structured, factual.
- Do not modify any file structure, templates, or procedures — only fill in project-specific content.
