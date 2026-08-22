# Shared Procedure: Design Gap

_Included by `SDLC-execute-task` and `SDLC-fix`. Read when the including skill directs here._

A design gap is a divergence between **design documents** and what implementation requires. When updating design artifacts as part of this procedure, follow the procedures and instructions in `.claude/skills/SDLC-design/SKILL.md` — in particular the **Modifying Existing Design Documents** section (downstream effect checks, present changes, wait for confirmation), **Decision Triggers** (record new decisions when design choices are approved), **Current State Tracking** (update `CLAUDE.md`), and the cross-skill artifact procedures defined in `CLAUDE.md`.

**Minor divergence** (field renamed, type made more specific, optional field added): update the relevant `2-design/` file following the SDLC-design modification procedures, continue.

**Significant divergence** (new endpoint/entity, invalid architectural assumption, requirement that cannot be implemented as designed):

1. **Stop** — do not write implementation code.
2. **Surface** the gap: what the design says, what implementation needs, why they differ.
3. **Present options**: update design first, simplify to stay within design, or accept and record deviation as a decision.
4. **Wait for explicit approval** before proceeding.
5. **Act**: update `2-design/` or `1-spec/` as needed following the SDLC-design procedures (including downstream effect checks, decision recording, and Current State tracking), then implement.
