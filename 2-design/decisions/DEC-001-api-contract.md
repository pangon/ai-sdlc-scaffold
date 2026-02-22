# DEC-001: API Contract Design

**Status**: Active
**Category**: Convention
**Scope**: system-wide
**Last updated**: 2026-02-22

## Context

When a project has separate API-interacting components (e.g. a frontend and a backend), both sides maintain their own type definitions. Without explicit rules, these definitions drift apart over time, causing runtime mismatches that are hard to detect and difficult for AI assistants to reason about.

## Decision

Adopt explicit contract rules with mirrored types. Every API endpoint has a named response type derived from internal entity types, never the entity itself. Consumer types mirror producer response types exactly. A standardized response envelope wraps all responses.

## Enforcement

### Trigger conditions

- **Design phase**: when specifying or changing an API endpoint, request/response type, or response envelope.
- **Code phase**: when creating or modifying an endpoint handler, service return type, API client call, or consumer component.

### Required patterns

- **Named response types**: every endpoint must have an explicit response type derived from the internal entity type, not the entity itself.
- **Mapper functions**: handlers convert entities to response types via mapper functions; they never return raw entities.
- **Mirrored consumer types**: field names, types, optionality, and enum values must match exactly between producer and consumer.
- **Response envelope**: all responses use a consistent wrapper (e.g. `{ success: true, data: {...} }`). The design spec documents the `data` payload only.
- **Named list fields**: list endpoints return `{ users: [...] }`, `{ products: [...] }` — never `{ items: [...] }`.

### Required checks — design phase

When specifying or modifying an API endpoint or response shape:

1. Confirm every endpoint has a named response type in the spec.
2. Confirm the response type excludes internal fields (e.g. hashed passwords, internal IDs not surfaced to clients).
3. Confirm the response envelope is consistent with existing endpoints.
4. Confirm list endpoints use named fields.
5. Update `2-design/api-design.md` to reflect the change.

### Required checks — code phase

When creating or modifying an endpoint handler, service return type, API client call, or consumer component:

1. Define or update the backend response type — derive from entity type, strip internal fields, add computed fields.
2. Ensure the handler uses a mapper function; never returns a raw entity.
3. Read the corresponding frontend type — compare field names, types, optionality, and enum values.
4. Sync the frontend type to match exactly.
5. Update the frontend API client (endpoint path, HTTP method, request body, return type).
6. Search for consumers of the modified type; verify they access correct fields.
7. Run typecheck on both sides.
8. If the API surface changed, update `2-design/api-design.md`.

### Prohibited patterns

- Returning raw internal entities from handlers.
- Using generic `{ items: [...] }` for list responses.
- Using `string` where a union type (`'active' | 'inactive'`) is known.
- Adding a new status value on one side without updating the other.
- Mismatched optionality between backend response type and frontend consumer type.
