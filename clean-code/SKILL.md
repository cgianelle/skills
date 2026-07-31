---
name: clean-code
description: Apply Clean Code craft while writing or reviewing code — meaningful names, small functions, comments, error handling, tests, classes, and smell heuristics from Robert C. Martin's Clean Code. Use when the user invokes clean-code, asks for a Clean Code review, or wants code rewritten/refactored to Clean Code standards.
disable-model-invocation: true
---

# Clean Code

Principles distilled from *Clean Code* by Robert C. Martin; not a verbatim reproduction of the book.

## Overview

Dual-mode skill for day-to-day craft: leave code cleaner than you found it (Boy Scout Rule), prefer clarity over cleverness, and make intent obvious in the code itself.

## Mode selection

At the start, detect intent or ask once:

| Mode | When | Behavior |
| --- | --- | --- |
| **Write** | Implementing, refactoring, or rewriting | Apply rules while changing code |
| **Review** | Critique requested | Report findings; do not rewrite unless asked |

If both apply (e.g. "review then fix"), Review first, then Write after the user confirms.

## Ground rules

Always on in both modes:

- Intention-revealing names; one word per concept
- Functions small, do one thing, one abstraction level; Stepdown Rule (read top → bottom)
- Prefer code that needs no comments; comments only for intent, warnings, TODOs, or public API
- Exceptions over error codes; avoid returning or passing null when a better type exists
- Tests F.I.R.S.T.; keep tests clean; one concept per test
- Classes small; SRP; high cohesion
- DRY; separate command from query
- Match existing project style when it conflicts with formatting minutiae

## Write workflow

1. Read surrounding code; match local patterns and naming.
2. Apply checklists in order: **Names → Functions → Errors → Structure → Tests** (as relevant).
3. Boy Scout: small cleanups only in files you already touch. No drive-by refactors of unrelated code.
4. Prefer extract/rename over explanatory comments.
5. Keep diffs focused on the requested change plus local cleanliness.

## Review workflow

1. Scan the change or target files against the core checklists and [smells.md](smells.md).
2. Tag each finding: **Must fix** / **Should fix** / **Nit**.
3. Group by area: Names, Functions, Comments, Errors, Tests, Classes, Smells.
4. Cite heuristic IDs when applicable (e.g. `G30`, `N1`).
5. Summarize top issues first; keep nits brief.
6. Offer a rewrite only if the user asks.

## Core checklists

### Names

- Do: reveal intent; pronounceable and searchable; meaningful distinctions; problem- or solution-domain vocabulary as appropriate.
- Don't: disinformation, mental mapping, encodings/Hungarian, noise words (`data`, `info`, `Manager` without meaning), cute or punny names, multiple words for one concept.

### Functions

- Do: small; one thing; one level of abstraction; descriptive verb names; few arguments (prefer 0–2); Stepdown Rule.
- Don't: flag arguments; output arguments; hidden side effects; mix command and query; giant switch/if trees that belong in polymorphism or dispatch tables (when the codebase supports it).

### Comments

- Do: legal headers; warn of consequences; clarify non-obvious intent; TODOs with context; public API docs when required.
- Don't: comment bad code instead of fixing it; redundant narration; journal/attribution noise; commented-out code; closing-brace labels; HTML in source comments.

### Formatting

- Do: newspaper metaphor (high-level first); related concepts vertically close; consistent team/project rules; readable horizontal density.
- Don't: huge files with unrelated concepts mixed; wild horizontal alignment schemes that fight diffs.

### Objects and data structures

- Do: clear boundary — objects hide data and expose behavior; data structures expose data and have little behavior.
- Don't: hybrids that half-hide and half-expose; train wrecks / Law of Demeter violations (`a.getB().getC().do()`).

### Error handling

- Do: exceptions for exceptional flow; try/catch/finally (or language equivalent) structured first; provide context; define exceptions for the caller’s needs; extract try/catch bodies into functions.
- Don't: return codes as the primary error channel; pass/return null to signal failure when Option/Result/empty collections fit better.

### Boundaries

- Do: wrap third-party APIs at clear edges; learning tests for unfamiliar libraries; keep boundary adapters thin.
- Don't: leak third-party types and quirks deep into domain code.

### Tests

- Do: F.I.R.S.T. (Fast, Independent, Repeatable, Self-validating, Timely); clean, readable tests; one concept per test; cover boundaries.
- Don't: dirty or duplicated test code; skipped “trivial” cases that document behavior; slow coupled suites that discourage running tests.

### Classes

- Do: small; single responsibility; high cohesion; organize for change; hide internals.
- Don't: god classes; low cohesion “utility dumps”; base classes that depend on derivatives.

### Emergence (simple design)

Aim for design that:

1. Runs all the tests
2. Contains no duplication
3. Expresses intent
4. Minimizes the number of classes and methods

Apply in that priority order when trade-offs appear.

## Out of scope

- System structure, packaging, and architectural boundaries → use the `architect` skill (and Clean Architecture material when available).
- Deep concurrency design → only flag obvious craft smells unless the user asks for more.

## Additional resources

- For the Ch. 17 smell and heuristic catalog, see [smells.md](smells.md).
