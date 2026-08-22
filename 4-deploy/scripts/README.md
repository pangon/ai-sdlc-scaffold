# Scripts

Deployment and operational scripts (deploy, rollback, backup, maintenance), created during Code-phase tasks that touch deployment (see [`../CLAUDE.deploy.md`](../CLAUDE.deploy.md)).

Every script must be idempotent, exit on failure with a clear error message, and provide a rollback path or document why one is not possible.
