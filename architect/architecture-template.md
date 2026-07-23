# Architecture Template

Use this structure. Omit sections that are genuinely irrelevant rather than filling them with filler. Prefer a lean spine of invariants over a comprehensive design encyclopedia.

```markdown
# Architecture: [System / Feature Name]

**Status:** Draft | In Review | Final
**Altitude:** System | Feature | Epic/Slice
**Paradigm:** [e.g. modular monolith, hexagonal, event-driven]
**Author:** [name]
**Last updated:** [date]
**Sources:** [PRDs, specs, prior ADRs — links or paths]

## 1. Purpose & Scope

What this document governs, who it is for, and what it deliberately does not decide.

## 2. Design Paradigm

Named approach and the model it implies (boundaries, dependency direction, how work is packaged). One short paragraph plus the implication for implementers.

## 3. Context & Constraints

- Brownfield conventions to ratify (or "greenfield")
- Hard constraints (cloud, language, compliance, timeline, team skills)
- External systems and integration edges

```mermaid
C4Context or simple flowchart of system + actors + external deps
```

## 4. Architecture Decisions

### AD-1 — [Decision title]

- **Status:** Proposed | Adopted | Superseded by AD-n
- **Binds:** [capabilities, FR/NFR IDs, areas, or `all`]
- **Prevents:** [the incompatible choices this stops]
- **Rule:** [the constraint downstream must follow]
- **Alternatives considered:** [A vs B vs C — one line each]
- **Notes:** optional; keep short. Tag `[ASSUMPTION]` if unconfirmed.

### AD-2 — …

Decisions that only appear in a diagram still get an AD entry.

## 5. Consistency Conventions

Cross-cutting rules that are not full ADs but must stay consistent (naming, error shape, auth pattern, logging, ID strategy, API style, etc.).

| Convention | Rule |
|------------|------|
| … | … |

## 6. Stack & Versions

Pin only what must not drift. Verify versions before writing them.

| Component | Choice | Version | Notes |
|-----------|--------|---------|-------|
| Language / runtime | | | |
| Framework / starter | | | |
| Data store | | | |
| … | | | |

## 7. Structural Seed

Minimal shape to start from — owned by the codebase once it exists. Keep terse.

```text
{root}/
  {dir}/    # {what lives here}
```

Optional Mermaid for runtime/deployment view when helpful.

## 8. Data & State

- Ownership: who writes what
- Shared data and consistency expectations
- Key entities at this altitude (not a full schema dump)

## 9. Quality Attributes

Only real, measurable constraints — omit dimensions with no requirement.

| Attribute | Requirement | Notes |
|-----------|-------------|-------|
| Performance | e.g. p95 … | |
| Availability | | |
| Security / privacy | | |
| Tenancy | | |
| Observability | | |

## 10. Delivery & Operations

Environments, deploy model, config/secrets approach, migration/rollback posture — only what must be consistent across teams.

## 11. Deferred

What this altitude will not decide, and when to revisit.

| Item | Why deferred | Revisit when |
|------|--------------|--------------|
| … | … | … |

## 12. Open Questions & Assumptions

- **Open:** [question] — owner, needed by [when]
- **Assumed:** [assumption not yet confirmed] `[ASSUMPTION]`

## 13. Risks

| Risk | Impact | Mitigation |
|------|--------|------------|
| … | … | … |
```

## Writing guidance

- Fix **invariants**, not every implementation detail. If two implementers could choose differently without breaking consistency, prefer Deferred.
- Every `AD` must state what it **Binds**, what divergence it **Prevents**, and the **Rule** to follow.
- Keep `AD` IDs stable forever. Amend a Rule in place; add `AD-n` for new decisions; mark superseded rather than renumbering.
- Structural Seed is a cold-start aid, not a permanent map of the repo.
- Prefer diagrams for shape; prefer tables for decisions and pins.
- Requirements describe **what** (PRD); this doc describes **how we stay consistent** while building.
