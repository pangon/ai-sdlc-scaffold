# CLAUDE.deploy.md

Phase-specific instructions for the **Deploy** phase. Extends [../CLAUDE.md](../CLAUDE.md).

## Purpose

This phase handles **deployment and operations**. Focus on reliability, repeatability, and observability.

---

## Decisions Relevant to This Phase

<!-- Add rows as decisions are recorded. -->

| ID | Title | Trigger |
|----|-------|---------|

---

## AI Guidelines

### Infrastructure as Code

1. Check `2-design/` for architecture design docs.
2. Apply all decisions from the index above whose trigger conditions match.
3. Write declarative, idempotent configurations.
4. Document resource dependencies in comments or in `infrastructure/README.md`.
5. Flag non-obvious cost drivers to the user.
6. Never hardcode secrets — use environment variables or a secret manager.

### Deployment Scripts

1. Check the decisions index above before composing tooling commands.
2. Make every script idempotent.
3. Exit on failure, log the failed step, emit a clear error message.
4. Log every significant action with a timestamp.
5. Provide a rollback path or document why one is not possible.

### Runbooks

1. Use the [runbook template](runbooks/_template.md).
2. Reference specific deployment scripts and infrastructure resources.
3. Link back to requirements where relevant (e.g., availability targets from REQ-REL).
4. Cross-check procedures against actual scripts and infrastructure.
5. Keep procedures short — move detailed background into a separate document if needed.

### Common Decision Triggers
When a significant decision emerges, follow [CLAUDE.md — Decisions](../CLAUDE.md#when-recording-decisions). Common triggers: secret management, environment promotion rules, rollback procedures, IaC tooling, CI/CD conventions.

---

## Environment Configuration

<!-- Fill in environment-specific details. -->

| Environment | Purpose | Notes |
|-------------|---------|-------|
| Development | Local development | - |
| Staging | Pre-production testing | - |
| Production | Live system | - |

---

## Linking to Other Phases

- Infrastructure design comes from `2-design/`
- Deploys build artifacts from `3-code/`
- Operational requirements come from `1-objectives/`
