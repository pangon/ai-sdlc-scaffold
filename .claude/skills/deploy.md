# Skill: /deploy

Generate or update deployment artifacts.

## Instructions

You are working in the Deploy phase, creating or updating infrastructure-as-code, deployment scripts, and runbooks.

### Setup

1. Read `CLAUDE.md` (root instructions) and `4-deploy/CLAUDE.deploy.md` (phase instructions). Follow both throughout this session.
2. Read the Decisions index in `CLAUDE.deploy.md` and load any decisions whose trigger conditions apply.
3. Read `2-design/architecture.md` to understand the system architecture.
4. Check `3-code/tasks.md` to understand which components have been implemented (status `Done`).

### Workflow

1. **Ask the user** what deployment artifacts to work on:
   - `4-deploy/infrastructure/` — Infrastructure as Code (Terraform, CloudFormation, Docker, etc.)
   - `4-deploy/scripts/` — Deployment scripts
   - `4-deploy/runbooks/` — Operational runbooks (use `4-deploy/runbooks/_template.md`)

2. **Read relevant context**:
   - Architecture and data model from `2-design/`
   - Operational requirements from `1-objectives/` (especially `REQ-REL`, `REQ-SEC`, `REQ-SCA` prefixed requirements)
   - Constraints that affect deployment from `1-objectives/constraints/`
   - Environment configuration in `CLAUDE.deploy.md`

3. **Draft the artifacts** following `CLAUDE.deploy.md` guidelines:
   - **IaC**: declarative, idempotent, document dependencies, flag cost drivers, never hardcode secrets
   - **Scripts**: idempotent, exit on failure, log actions with timestamps, provide rollback path
   - **Runbooks**: use the template, reference specific scripts and resources, link to requirements, keep procedures short

4. **Present the draft** to the user before writing. Highlight:
   - Which requirements are addressed (especially reliability, security, scalability)
   - Cost drivers or resource implications
   - Rollback strategy
   - Any missing configuration or secrets that need to be provided

5. **After user approval**, write the files.

### Decision Triggers

Common triggers during deployment work (from `CLAUDE.deploy.md`):
- Secret management approach
- Environment promotion rules
- Rollback procedures
- IaC tooling choice
- CI/CD conventions

If a significant decision emerges, surface it and suggest recording it using the `/decide` skill.

### Rules

- Never hardcode secrets — use environment variables or a secret manager.
- All scripts must be idempotent.
- Flag non-obvious cost drivers to the user.
- Runbooks must reference specific scripts and infrastructure resources.
- Link back to requirements where relevant (e.g., availability targets from `REQ-REL`).
