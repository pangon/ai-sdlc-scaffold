# CLAUDE.deploy.md

Phase-specific instructions for the **Deploy** phase. Extends [../CLAUDE.md](../CLAUDE.md).

## Purpose

This phase handles **deployment and operations**. Focus on reliability, repeatability, and observability.

## Directory Structure

```
4-deploy/
├── CLAUDE.deploy.md  # This file
├── infrastructure/   # Infrastructure as Code (IaC)
├── scripts/          # Deployment and utility scripts
└── runbooks/         # Operational procedures
```

---

## Decisions relevant to the deploy phase

| ID | Title | Trigger |
|----|-------|---------|
| [DEC-002](../2-design/decisions/DEC-002-nodejs-version-management.md) | Node.js Version Management | When running any Node.js tooling command (`npm`, `npx`, `node`, `tsc`, etc.) in a deploy script or CI configuration in a directory with a `.nvmrc` file |

---

## AI Guidelines for This Phase

### When Writing Infrastructure as Code

1. Check requirements in `1-objectives/` for operational and compliance constraints (REQ-REL, REQ-SEC, REQ-COMP).
2. Check `2-design/` for infrastructure decisions referenced in the architecture or design docs.
3. Apply all decisions from the index above whose trigger conditions match.
4. Write declarative configurations where possible; prefer idempotent resources.
5. Document resource dependencies in comments or in `infrastructure/README.md`.
6. Consider cost implications; flag non-obvious cost drivers to the user.
7. Never hardcode secrets — use environment variables or a secret manager.

### When Writing Deployment Scripts

1. Check the decisions index above before composing any tooling commands.
2. Make every script idempotent (safe to run multiple times without side effects).
3. Include error handling: exit on failure, log the failed step, and emit a clear error message.
4. Log every significant action with a timestamp so failures are diagnosable.
5. Provide a rollback path or document explicitly why one is not possible.
6. After writing the script, verify it references the correct artifact paths from `3-code/`.

### When Writing Runbooks

1. Use the runbook template below — do not skip any section.
2. Reference the specific deployment scripts and infrastructure resources the runbook covers.
3. Link back to requirements where relevant (e.g., availability targets from REQ-REL).
4. After writing a runbook, cross-check the procedure against the actual scripts and infrastructure to confirm the steps are accurate.
5. Keep procedures short — move detailed background into a separate document if needed.

### When Recording Decisions
When a significant decision is made during this phase, record it as explained in [CLAUDE.md — Decisions](../CLAUDE.md#decisions).

Common triggers for new decisions in this phase: secret management strategy, environment promotion rules, rollback procedures, IaC tooling choices, CI/CD pipeline conventions.

---

## Runbook Template

```markdown
# Runbook: [Operation Name]

## Overview
Brief description of what this runbook covers.

## Prerequisites
- Required access/permissions
- Required tools
- Required information

## Procedure

### Step 1: [Action]
```bash
# command to run
```
Expected outcome: ...

### Step 2: [Action]
...

## Rollback Procedure
Steps to undo if something goes wrong.

## Troubleshooting
Common issues and their solutions.
```

---

## Environment Configuration

Document environment-specific configurations:

| Environment | Purpose | Notes |
|-------------|---------|-------|
| Development | Local development | - |
| Staging | Pre-production testing | - |
| Production | Live system | - |

---

## Linking to Other Phases

- Infrastructure design comes from `2-design/`
- Deploys build artifacts from `3-code/`
- Operational requirements (availability, compliance) come from `1-objectives/`
