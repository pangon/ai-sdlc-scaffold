# Skill: /fix

Bug-fix workflow following the structured procedure.

## Instructions

You are fixing a bug using the structured procedure defined in `CLAUDE.code.md`.

### Setup

1. Read `CLAUDE.md` (root instructions) and `3-code/CLAUDE.code.md` (phase instructions). Follow both throughout this session.
2. Read the Decisions index in `CLAUDE.code.md` and load any decisions whose trigger conditions match.

### Workflow (from CLAUDE.code.md § Fixing Bugs)

1. **Understand the bug**: ask the user to describe the bug if not already clear. Identify the affected component and expected vs actual behavior.

2. **Write a failing test**: create a test that reproduces the bug. Run it to confirm it fails. This test documents the bug and prevents regressions.

3. **Fix the bug**: make the minimal change needed to fix the issue. Verify the failing test now passes.

4. **Check decisions**: if the bug indicates a violation of an active decision (from the decisions index), apply the full enforcement procedure from that decision's Required checks.

5. **Search for the same pattern**: search the codebase for the same bug pattern elsewhere. If found, **ask the user** whether to fix those occurrences too. Do not fix them silently.

6. **Update tasks**: if this bug fix corresponds to an existing task in `tasks.md`, update its status. If no task exists and the fix is significant, consider creating one for traceability.

### Rules

- Always write a failing test first, before fixing.
- Make the minimal change needed — do not refactor surrounding code.
- If the same pattern exists elsewhere, always ask before fixing other occurrences.
- Follow the code quality guidelines from `CLAUDE.code.md`.
- Run the full test suite after fixing to ensure no regressions.
