# PRD Template

Use this structure. Omit sections that are genuinely irrelevant rather than filling them with filler.

```markdown
# PRD: [Feature / Product Name]

**Status:** Draft | In Review | Approved
**Author:** [name]
**Last updated:** [date]

## 1. Problem Statement

What problem exists, who has it, and why it matters now. 1–2 paragraphs, no solutioning.

## 2. Goals & Success Metrics

| Goal | Metric | Target |
|------|--------|--------|
| [outcome we want] | [how it's measured] | [number + timeframe] |

Non-goals: what this effort deliberately does not try to achieve.

## 3. Users & Use Cases

- **Primary user:** [who] — [key use case]
- **Secondary user:** [who] — [key use case]

What they do today instead (current workaround or competitor).

## 4. Functional Requirements

| ID | Requirement | Priority | Notes |
|----|-------------|----------|-------|
| FR-1 | [testable statement of what the system must do] | P0 | |
| FR-2 | ... | P1 | |

Priorities: P0 = MVP blocker, P1 = should have, P2 = nice to have.

## 5. Non-Functional Requirements

Performance, security, privacy, accessibility, reliability, localization — only the ones with real constraints, stated measurably (e.g. "p95 page load < 2s").

## 6. Scope

### In scope (MVP)
The smallest coherent version that delivers the core value.

### Out of scope / Future
Everything deferred, with a one-line reason each.

## 7. Dependencies & Constraints

External teams, systems, deadlines, budget, compliance requirements.

## 8. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|

Include the riskiest assumption and how it will be validated.

## 9. Open Questions & Assumptions

- **Open:** [question] — owner, needed by [when]
- **Assumed:** [assumption the user has not confirmed]

## 10. Release & Rollout (optional)

Phasing, feature flags, migration, success checkpoint before broader rollout.
```

## Writing guidance

- Requirements describe **what**, not **how**. Implementation details belong in a tech design doc.
- One requirement per row. If a row contains "and", consider splitting it.
- If a metric target is unknown, write "TBD — [who decides]" rather than inventing a number.
