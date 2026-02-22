# CLAUDE.objectives.md

Phase-specific instructions for the **Objectives** phase. Extends [../CLAUDE.md](../CLAUDE.md).

## Purpose

This phase defines **what** we're building and **why**. Focus on clarity, measurability, and alignment with user needs.

## Files in This Phase

| File | Purpose |
|------|---------|
| `goals.md` | High-level project goals and success criteria |
| `requirements.md` | Functional and non-functional requirements |
| `constraints.md` | Technical, business, and resource constraints |

## AI Guidelines for This Phase

### When Defining Goals
- Ask clarifying questions to understand the user's vision
- Help decompose vague ideas into concrete, measurable objectives
- Identify potential conflicts between goals early

### When Writing Requirements
- Use clear, testable language (avoid "should be fast" -> use "response time < 200ms")
- Distinguish between must-have and nice-to-have features
- Link requirements back to goals they support

### When Documenting Constraints
- Consider technical constraints (platforms, languages, dependencies)
- Consider business constraints (budget, timeline, team size)
- Consider operational constraints (hosting, maintenance, compliance)

## Linking to Other Phases

- Goals and requirements should be referenced in design documents (`02-design/`)
- Acceptance criteria from requirements inform test cases (`03-code/tests/`)
- Constraints influence infrastructure decisions (`04-deploy/`)

## Templates

When creating new documents in this phase, Claude should use appropriate structure:

### Goal Template
```markdown
## Goal: [Name]

**Description**: What we want to achieve

**Success Criteria**:
- [ ] Measurable outcome 1
- [ ] Measurable outcome 2

**Priority**: High / Medium / Low

**Related Requirements**: [links]
```

### Requirement Template
```markdown
## REQ-[ID]: [Title]

**Type**: Functional / Non-functional
**Priority**: Must-have / Should-have / Nice-to-have
**Related Goal**: [link]

**Description**: Detailed requirement description

**Acceptance Criteria**:
- Given [context], when [action], then [outcome]
```
