# DEC-002: Node.js Version Management

**Status**: Active
**Category**: Convention
**Scope**: system-wide
**Last updated**: 2026-02-22

## Context

This project uses **nvm** with `.nvmrc` files to pin Node.js versions per sub-project. nvm does not auto-activate in non-interactive shells — the kind used by AI agents when running Bash commands. Commands run without sourcing nvm will silently use the system Node.js, which may be an incompatible version and produce incorrect or misleading results.

## Decision

Every Bash command that invokes Node.js tooling (`npm`, `npx`, `node`, `tsc`, etc.) in a directory that contains a `.nvmrc` file must be prefixed with:

```bash
source ~/.nvm/nvm.sh && nvm use &&
```

## Enforcement

### Trigger conditions

- **Code phase**: when running any Node.js tooling command (`npm`, `npx`, `node`, `tsc`, etc.) in a directory with a `.nvmrc` file.

### Required patterns

All Node.js tooling commands in `.nvmrc` directories must use the prefix:

```bash
# Correct
source ~/.nvm/nvm.sh && nvm use && npm run typecheck

# Correct — single prefix covers chained commands
source ~/.nvm/nvm.sh && nvm use && npm run lint && npm run typecheck
```

### Required checks

Before composing any Node.js tooling command:

1. Check whether the target directory contains a `.nvmrc` file.
2. If yes, prepend `source ~/.nvm/nvm.sh && nvm use &&` to the command.

### Prohibited patterns

- Running `npm`, `npx`, `node`, or `tsc` directly in a `.nvmrc` directory without the nvm prefix.
