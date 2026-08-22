# Shared Procedure: Constraint Tension Check

_Included by `SDLC-execute-task` and `SDLC-fix`. Read when the including skill directs here._

First, load the active constraints: read the Constraints Index in `1-spec/CLAUDE.spec.md` and open any constraint whose summary is plausibly relevant to the work at hand (technology, performance, security, compliance, hosting, timeline, …) — tensions cannot be detected against constraints that were never read.

Then, with all gathered context (the elicited problem or task scope, requirements, design documents, decisions, constraints, phase instructions), actively look for **tensions between authoritative sources**. A tension exists when two or more sources that the agent must follow pull in incompatible directions — satisfying one fully would require violating or bending another.

Common tension patterns:

- **Architectural decisions vs. component isolation rules** — e.g., a decision requiring shared state across components while isolation rules prohibit cross-component coupling.
- **Cross-cutting decisions vs. per-component conventions** — e.g., a system-wide decision that conflicts with a component-specific pattern.
- **Requirements vs. constraints** — e.g., a functional requirement that is difficult to satisfy within a declared constraint.
- **Design document assumptions vs. phase instructions** — e.g., a design that assumes a project structure incompatible with the code phase conventions.

**If a tension is found, stop and surface it to the user before implementing.** Present:

1. **The conflicting sources** — cite both by name and quote the relevant rules.
2. **Why they conflict** — explain the specific scenario where following one source requires bending the other.
3. **Options** — propose at least two resolution paths (e.g., adjust one source, record a scoped exception as a decision, restructure the approach).
4. **Wait for the user's decision** before proceeding.

A tension is **not** the same as a design gap (divergence between design and implementation needs). Tensions are contradictions *within* the set of authoritative sources. They are an "always ask" tier situation — the agent must never resolve them silently, because choosing one source over another is a project-level decision that belongs to the user.
