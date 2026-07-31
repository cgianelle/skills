---
name: context-engineering
description: >-
  Design, structure, and write project context files (AGENTS.md, .cursor/rules)
  so AI coding assistants generate code aligned with architecture, conventions,
  and domain constraints. Use when starting a project, bootstrapping AI context,
  refreshing stale context, or when the user asks for context engineering,
  AGENTS.md, Cursor rules setup, or ContextOps for a repo.
disable-model-invocation: true
---

# Context Engineering

## Overview

Turn implicit project knowledge into durable, AI-usable context files. Context beats bigger models: without structured architecture, conventions, and domain constraints, assistants guess — and rework rises.

This skill has two intents:

1. **Bootstrap** — greenfield or first-time context setup for a repo
2. **Refresh** — audit and update existing context against the current codebase

Default deliverables (write to the project unless the user redirects):

| Artifact | Role |
| --- | --- |
| `AGENTS.md` | Operational brief for any coding agent — what the system is, layout, stack, commands, invariants |
| `.cursor/rules/*.mdc` | Focused Cursor rules — conventions, patterns, domain/security guardrails |

Do **not** dump everything into one mega-file. Prefer a lean `AGENTS.md` plus small, single-concern rules.

## Ground Rules

- Discover and audit before writing. Never invent conventions the codebase doesn't use.
- **Token efficiency**: every line must earn its place. Prefer hierarchies, short examples, and "do / don't" over essays.
- One concern per rule file. Keep each `.mdc` under ~50 lines when possible.
- Distinguish facts from assumptions. Tag guesses `[ASSUMPTION]` or put them under Open Questions.
- Ratify brownfield reality; don't redesign architecture under the guise of context (hand that to `architect`).
- Never put secrets, credentials, or private keys in context files.
- Ask in small batches (2–4 questions). Use the AskQuestion tool when available.
- Offer Coaching vs Fast path once at the start if the user hasn't chosen.

## Working Modes

1. **Coaching path (default)** — interview, play back, then write
2. **Fast path** — draft from repo scan with `[ASSUMPTION]` tags for review

Unless the user asks for speed, coach.

## The Five Context Types

Cover each type — include, defer, or mark N/A. Do not silently skip.

| Type | What to capture | Typical home |
| --- | --- | --- |
| **Architectural** | Paradigm, boundaries, ownership, communication rules, target vs legacy | `AGENTS.md` + optional always-apply rule |
| **Codebase** | Layout, critical modules/APIs, stack, how to run/test | `AGENTS.md` |
| **Business domain** | Business rules, regulatory constraints, non-negotiable invariants | Dedicated rule or `AGENTS.md` Invariants |
| **Historical** | Rejected approaches, ADRs, migration direction, "why not X" | Short section in `AGENTS.md` or linked ADR index — not commit dumps |
| **Execution** | Envs, deploy shape, config that changes behavior | `AGENTS.md` Setup & commands |

## Workflow

### Step 0: Choose intent and path

Confirm:

1. **Bootstrap** or **Refresh**
2. Coaching or Fast
3. Project root (move agent root if needed)

### Step 1: Audit existing context sources

Scan before interviewing:

- `AGENTS.md`, `CLAUDE.md`, `.cursorrules`, `.cursor/rules/**`
- `README.md`, `docs/**`, ADRs, architecture docs
- Package manifests, make/just/task files, CI configs
- Existing skills/rules that already encode conventions

For **Refresh**: list what exists, flag stale/contradictory/bloated sections, and propose a diff plan before rewriting.

For **Bootstrap**: note gaps and which of the five types are missing.

### Step 2: Discovery (skip what's already known)

Cover in batches:

1. **Job of context** — Who consumes it (Cursor only vs multi-agent)? Any compliance/regulated constraints?
2. **Architecture** — Paradigm, major boundaries, ownership seams, legacy vs target
3. **Conventions** — Naming, error handling, testing, PR/review norms worth encoding
4. **Patterns & anti-patterns** — 3–7 project-specific do/don'ts (highest leverage)
5. **Domain invariants** — Rules that must never be violated in generated code
6. **Critical modules** — Paths agents must know; public APIs; "do not touch" zones
7. **Historical landmines** — Approaches already rejected and why (brief)
8. **Execution** — How to run, test, and which env vars matter (no secret values)

If an `architect` doc or PRD already settles items, inherit them — don't re-decide.

### Step 3: Play back the context plan

Before writing files, summarize:

- Intent (bootstrap/refresh)
- Files to create or update
- Which of the five types land where
- Explicitly deferred (to avoid bloat)
- Open questions / assumptions

Get confirmation on Coaching path.

### Step 4: Prioritize the high-impact set

Ship the smallest set that moves quality:

1. `AGENTS.md` — always
2. One always-apply core standards rule (if conventions aren't fully covered in `AGENTS.md`)
3. 1–3 focused rules for hot zones (e.g. `**/api/**`, payments, auth) when glob-scoped guidance helps

Avoid creating rules for every directory. Add more only when refresh feedback shows gaps.

Use templates in [templates.md](templates.md).

### Step 5: Write artifacts

#### AGENTS.md

- Operational brief, not a second README encyclopedia
- Point to `README.md` / architecture docs for deep detail
- Include: what it is, layout & ownership, stack, setup/commands, invariants, agent do/don't
- Keep historical context to a short "Decisions / avoid" list with links to ADRs when they exist

#### `.cursor/rules/*.mdc`

Frontmatter:

```yaml
---
description: Short picker description
globs: **/*.ts          # omit or leave empty when alwaysApply
alwaysApply: false      # true only for universal standards
---
```

- `alwaysApply: true` — only for truly global, short standards
- Prefer `globs` for stack- or area-specific guidance
- Actionable do/don't with one concrete example when helpful
- No marketing fluff; no duplicate of `AGENTS.md` content

### Step 6: Anti-bloat and freshness check

Before presenting:

- [ ] Would removing any section lose a decision an agent would otherwise guess wrong?
- [ ] Any file > ~150 lines of AGENTS or > ~50 lines of a rule without clear need? Compress or split.
- [ ] Contradictions between `AGENTS.md`, rules, and README resolved?
- [ ] No secrets or environment-specific credentials?
- [ ] Refresh: obsolete APIs/patterns removed or marked legacy?
- [ ] Five context types each addressed, deferred, or N/A?

### Step 7: Handoff

Report:

- Paths written or updated
- What was deferred and why
- Suggested next: wire file-specific rules after first real feature work, or run **Refresh** after major architecture changes

Optional curation note for the user: treat context like code — update when architecture or conventions change; prefer small PRs that amend context alongside the change that invalidates it.

## Refresh-Specific Guidance

When refreshing:

1. Diff context claims against the repo (layout, commands, stack versions, invariants)
2. Remove or rewrite hallucinated / obsolete guidance first — deletions beat additions
3. Add only gaps proven by recent pain (wrong patterns, repeated review nits)
4. Preserve stable section structure in `AGENTS.md` when possible so humans recognize the doc
5. Do not renumber or scatter guidance across new files without a reason

## Collaboration

- System design / invariants at architecture altitude → `architect`
- Product scope / requirements → `product-manager`
- UX flows → `ux-designer`
- Creating a single ad-hoc Cursor rule outside this workflow → create-rule skill
- Do not refactor application code unless asked; context files only

## Additional Resources

- File templates and examples: [templates.md](templates.md)
- Source concepts: Packmind — [Context Engineering: The Complete Guide](https://packmind.com/context-engineering-ai-coding/)
