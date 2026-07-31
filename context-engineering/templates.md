# Context Engineering Templates

Use these as starting points. Cut ruthlessly to fit the project. Prefer facts verified from the repo over placeholders.

---

## AGENTS.md

```markdown
# Agent guide — [project-name]

Instructions for AI coding agents. Human overview and deep architecture live in `README.md` (and `docs/` if present). Keep this file as the operational brief.

## What this is

[1–3 sentences: product purpose and primary runtime shape]

## Layout & ownership

\`\`\`
[dir]/               [responsibility]
[dir]/               [responsibility]
\`\`\`

- **[Owner A] owns** …
- **[Owner B] owns** …
- **Do not** …

## Stack (ratified)

| Piece | Choice |
| --- | --- |
| Language | … |
| Framework | … |
| Data | … |
| Tests | … |

## Setup & commands

\`\`\`bash
# install
# run
# test
# lint / typecheck
\`\`\`

Env vars that change behavior (names only — never secret values): `…`

## Invariants

1. …
2. …
3. …

## Agent do / don't

**Do**
- …

**Don't**
- …

## Decisions / avoid (historical)

- Prefer X over Y because … (link ADR if any)
- Rejected Z because …

## Open questions

- …
```

---

## Always-apply core standards rule

Path: `.cursor/rules/core-standards.mdc`

```markdown
---
description: Core coding standards for this project
alwaysApply: true
---

# Core standards

## Style
- …

## Errors
- …

## Testing
- …

## PRs
- …
```

Keep this short. Move stack-specific detail into glob-scoped rules.

---

## Glob-scoped pattern rule

Path: `.cursor/rules/[area]-patterns.mdc`

```markdown
---
description: [Area] patterns and anti-patterns
globs: [glob]
alwaysApply: false
---

# [Area] patterns

## Do
- …

## Don't
- …

## Example

\`\`\`[lang]
// preferred
\`\`\`
```

---

## Domain / compliance rule

Path: `.cursor/rules/domain-invariants.mdc`

Use when business or regulatory rules must constrain generated code.

```markdown
---
description: Non-negotiable domain and compliance invariants
alwaysApply: true
---

# Domain invariants

- …
- …

## Forbidden
- …
```

If invariants only apply under certain paths (e.g. payments), prefer `globs` over always-apply.

---

## Minimal bootstrap set

For a first pass, create at most:

1. `AGENTS.md`
2. `.cursor/rules/core-standards.mdc` (only if conventions aren't already clear in AGENTS.md)
3. One area rule if a hot path needs tighter guidance

Add more rules only after Refresh finds real gaps.
