---
name: clean-code
description: Apply Clean Code, TDD, SOLID, functional design, and Fowler refactoring while writing, reviewing, or restructuring code. Use when the user invokes clean-code, asks for a Clean Code / SOLID / TDD / refactoring review, wants test-first implementation, or wants behavior-preserving refactoring.
disable-model-invocation: true
---

# Clean Code

Craft distilled from Robert C. Martin (*Clean Code*, *Clean Craftsmanship*, *Agile Software Development*, *Functional Design*) and Martin Fowler (*Refactoring*). Not a verbatim reproduction of those sources.

## Overview

Leave code cleaner than you found it (Boy Scout Rule). Prefer clarity over cleverness. Make intent obvious in the code. Write tests first. Refactor in small steps with tests green between them.

## Mode selection

At the start, detect intent or ask once:

| Mode | When | Behavior |
| --- | --- | --- |
| **Write** | Implementing or rewriting behavior | Test-first; apply rules while changing code |
| **Review** | Critique requested | Report findings; do not rewrite unless asked |
| **Refactor** | Restructure existing behavior | Behavior-preserving only; two hats; no feature work |

If Review and Write both apply (e.g. "review then fix"), Review first, then Write after the user confirms.

## Ground rules

Always on in every mode:

- Intention-revealing names; one word per concept
- Functions small, do one thing, one abstraction level; Stepdown Rule (read top → bottom)
- Prefer code that needs no comments; comments only for intent, warnings, TODOs, or public API
- Exceptions over error codes; avoid returning or passing null when a better type exists
- Tests F.I.R.S.T.; keep tests clean; one concept per test
- Test-first: follow the three laws in [tdd.md](tdd.md). The only escape hatch is a repo with no runnable test setup — state that explicitly; never skip silently
- Classes small; SRP; high cohesion; SOLID at class/module scope ([solid.md](solid.md))
- DRY; separate command from query
- Immutability and purity by default; push side effects to edges ([functional-design.md](functional-design.md))
- Refactor in small steps with green tests between them; name each move by its catalog name
- Match existing project style when it conflicts with formatting minutiae

## Reference files

Load on demand — do not load all of them every run:

| Load | When |
| --- | --- |
| [smells.md](smells.md) | Review, or Write/Refactor when a heuristic ID is in play |
| [refactoring.md](refactoring.md) | Review; any restructuring of existing code |
| [refactoring-catalog.md](refactoring-catalog.md) | Choosing or applying a named refactoring |
| [tdd.md](tdd.md) | Write; adding or changing tests |
| [solid.md](solid.md) | Class/module design tension, growing switch/if trees, dependency direction |
| [functional-design.md](functional-design.md) | FP language, pipelines, heavy mutable state, or hybrid OO + immutability |

## Write workflow

1. Read surrounding code; match local patterns and naming.
2. If the repo has a test command, work test-first per [tdd.md](tdd.md): failing test → minimal production code → refactor. If there is no runnable test setup, say so and continue without inventing a framework unless asked.
3. Apply checklists in order: **Names → Functions → Errors → Structure → Tests**.
4. Boy Scout: small cleanups only in files you already touch. No drive-by refactors of unrelated code. Switch to **Refactor** mode (two hats) rather than mixing feature work with restructuring.
5. Prefer extract/rename over explanatory comments.
6. Keep diffs focused on the requested change plus local cleanliness.

## Refactor workflow

1. Confirm tests are green. If they are not, stop and say so — do not refactor on red.
2. Wear the refactoring hat only: no new behavior and no bug fixes mixed in (fix first or refactor first — never both).
3. Load [refactoring.md](refactoring.md) and [refactoring-catalog.md](refactoring-catalog.md). Pick the smell, then the named move.
4. One small step, run tests, keep them green. Revert the step if tests go red.
5. Name the moves you applied in the return summary.

## Review workflow

1. Scan the change or target files against the core checklists, [smells.md](smells.md), and [refactoring.md](refactoring.md). Load [solid.md](solid.md) / [functional-design.md](functional-design.md) / [tdd.md](tdd.md) when those lenses apply.
2. Tag each finding: **Must fix** / **Should fix** / **Nit**.
3. Group by area: Names, Functions, Comments, Errors, Tests, Classes, Smells, Design, Refactoring.
4. Cite heuristic IDs (`G30`, `N1`, `RS-7`, `SP-2`, …) and catalog names when proposing a move.
5. Summarize top issues first; keep nits brief.
6. Offer a rewrite only if the user asks.

## Core checklists

### Names

- Do: reveal intent; pronounceable and searchable; meaningful distinctions; problem- or solution-domain vocabulary. When a build package is present, use its ubiquitous language for identifiers.
- Don't: disinformation, mental mapping, encodings/Hungarian, noise words, punny names, multiple words for one concept, or a synonym invented instead of a package term — flag the conflict.

### Functions

- Do: small; one thing; one level of abstraction; descriptive verb names; few arguments (prefer 0–2); Stepdown Rule.
- Don't: flag arguments; output arguments; hidden side effects; mix command and query; giant switch/if trees that belong in polymorphism or dispatch (when the codebase supports it).

### Comments

- Do: legal headers; warn of consequences; clarify non-obvious intent; TODOs with context; public API docs when required.
- Don't: comment bad code instead of fixing it; redundant narration; journal noise; commented-out code; closing-brace labels.

### Formatting

- Do: newspaper metaphor (high-level first); related concepts vertically close; consistent team/project rules.
- Don't: huge files with unrelated concepts mixed; alignment schemes that fight diffs.

### Objects and data structures

- Do: objects hide data and expose behavior; data structures expose data and have little behavior.
- Don't: hybrids; train wrecks / Law of Demeter (`a.getB().getC().do()`).

### Error handling

- Do: exceptions for exceptional flow; structure try/catch first; provide context; extract try/catch bodies.
- Don't: return codes as the primary error channel; pass/return null to signal failure when Option/Result/empty collections fit better.

### Boundaries

- Do: wrap third-party APIs at clear edges; learning tests for unfamiliar libraries; keep adapters thin.
- Don't: leak third-party types and quirks deep into domain code.

### Tests

- Do: F.I.R.S.T.; clean tests; one concept per test; cover boundaries; test-first when a runner exists.
- Don't: dirty or duplicated test code; skipped “trivial” cases; slow coupled suites; interaction tests that freeze implementation.

### Classes

- Do: small; single responsibility; high cohesion; organize for change; hide internals.
- Don't: god classes; low-cohesion utility dumps; base classes that depend on derivatives.

### Simple design (Beck)

Aim for design that, in this priority order:

1. Runs all the tests
2. Contains no duplication
3. Expresses intent
4. Minimizes the number of classes and methods

See [tdd.md](tdd.md) for the cycles that produce this.

## Out of scope

- System structure, packaging, layering, and component/package principles (REP, CCP, CRP, ADP, SDP, SAP) → `architect`. Clean-code owns class-and-below SOLID and behavior-preserving refactoring.
- Product scope → `product-manager`. UX → `ux-designer`.
- Deep concurrency design → only flag obvious craft smells unless the user asks for more.
