# DEC-001: Trail

> Companion to `DEC-001-error-format.md`.
> AI agents read this only when evaluating whether the decision is still
> valid or when proposing a change or supersession.

## Alternatives considered

### Option A: Unstructured error strings
- Pros: zero overhead, no format to learn
- Cons: consumers cannot programmatically distinguish error types; localization is impossible; debugging requires log correlation

### Option B: Structured error object (chosen)
- Pros: machine-readable codes enable retry logic and UI mapping; human-readable message supports debugging; optional detail keeps the base format lean
- Cons: slight overhead in serialization; requires discipline to maintain code registry

### Option C: Error code library / shared package
- Pros: single source of truth for all error codes
- Cons: tight coupling between components; versioning overhead; doesn't work across language boundaries

## Reasoning

Option B balances structure with simplicity. The three-field format is language-agnostic and works for REST APIs, GraphQL, CLI tools, and inter-service communication. The optional `detail` field avoids bloating simple error responses. Option C was rejected because the scaffold is technology-agnostic and should not impose a shared package dependency.

## Human involvement

**Type**: ai-proposed/human-approved
**Notes**: This is a scaffold example decision. Replace or adapt it for your project.

## Changelog

| Date | Change | Involvement |
|------|--------|-------------|
| 2026-02-23 | Initial decision — scaffold example | ai-proposed/human-approved |
