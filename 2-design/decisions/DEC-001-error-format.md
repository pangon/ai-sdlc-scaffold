# DEC-001: Error Response Format

**Status**: Active
**Category**: Convention
**Scope**: system-wide
**Source**: n/a
**Last updated**: 2026-02-23

## Context

Without a consistent error format, each component invents its own structure. Consumers must handle multiple shapes. AI agents cannot predict error fields when implementing error handling or writing tests.

## Decision

All components return errors using a consistent structure with three fields:

- **code** (string, UPPER_SNAKE_CASE) — machine-readable error identifier
- **message** (string) — human-readable description
- **detail** (object, optional) — structured context for debugging

Example:

```json
{
  "code": "VALIDATION_FAILED",
  "message": "Email address is not valid.",
  "detail": { "field": "email", "value": "not-an-email" }
}
```

## Enforcement

### Trigger conditions

- **Design phase**: when specifying error responses for any API or interface
- **Code phase**: when implementing error handling or returning errors from any component

### Required patterns

- Every error response includes `code` and `message` at minimum.
- Error codes are UPPER_SNAKE_CASE strings, documented per component.
- The `detail` object, when present, contains only serializable values.

### Required checks

1. Confirm every error path returns the standard structure.
2. Confirm error codes are documented (in API design or component README).
3. Confirm no component returns raw exception strings or stack traces to consumers.

### Prohibited patterns

- Returning raw exception messages or stack traces to external consumers
- Using numeric-only error codes without string identifiers
- Inconsistent error shapes between components or endpoints
