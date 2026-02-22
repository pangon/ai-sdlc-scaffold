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

## AI Guidelines for This Phase

### Infrastructure as Code
- All infrastructure should be defined in code
- Use declarative configurations where possible
- Document resource dependencies
- Consider cost implications

### Deployment Scripts
- Make scripts idempotent (safe to run multiple times)
- Include proper error handling
- Log actions for debugging
- Support rollback procedures

### Runbooks
- Document step-by-step procedures for common operations
- Include troubleshooting guides
- Keep procedures up to date with system changes

### Security Considerations
- Never commit secrets or credentials
- Use environment variables or secret management
- Document required permissions and access
- Follow principle of least privilege

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
\`\`\`bash
# command to run
\`\`\`
Expected outcome: ...

### Step 2: [Action]
...

## Rollback Procedure
Steps to undo if something goes wrong.

## Troubleshooting
Common issues and their solutions.
```

## Environment Configuration

Document environment-specific configurations:

| Environment | Purpose | Notes |
|-------------|---------|-------|
| Development | Local development | - |
| Staging | Pre-production testing | - |
| Production | Live system | - |

## Linking to Other Phases

- Infrastructure design comes from `2-design/`
- Deploys build artifacts from `3-code/`
- Operational requirements may come from `1-objectives/`
