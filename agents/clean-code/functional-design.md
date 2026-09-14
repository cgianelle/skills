# Functional design

Distilled from Robert C. Martin, *Functional Design: Principles, Patterns, and Practices*. Not a verbatim reproduction. Cite as `FD-n`. SOLID still applies; restatements are below. Pair with [solid.md](solid.md) and mutable-data smell `RS-6`.

Immutability and purity are the default. Mutating OO is allowed when the language or framework demands it — isolate it, do not sprinkle it.

## Defaults

| ID | Rule | Flag when | Do instead |
| --- | --- | --- | --- |
| FD-1 | Immutability | Fields, structs, or collections change after creation; hidden setters; “update” mutates in place | Values are replaced, not edited. Prefer `const` / `final` / `readonly` / records / persistent collections. |
| FD-2 | Referential transparency | Same call, same args, different result because of time, I/O, or mutable cells | A function’s result depends only on its arguments. Clock, IO, and RNG are arguments or live outside the core. |
| FD-3 | Functional core, imperative shell | Domain rules perform I/O, mutate globals, or construct infrastructure | Pure functions compute; a thin shell reads, calls the core, writes. Adapters stay at the edge ([smells.md](smells.md) Boundaries). |
| FD-4 | Copy-on-write / persistent data | Shared structure is mutated; defensive copies are ad-hoc and incomplete | Update returns a new value. Share unchanged parts (persistent structures) when cheap; copy small values when not. |
| FD-5 | Compose functions | Nested loops, hand-rolled map/filter/reduce, or a long script of mutations | Name each step; pipe / compose / chain. Replace Loop with Pipeline when the language has one. |
| FD-6 | Side effects at the boundary | Logging, HTTP, disk, or process-wide state inside a calculation | Push effects to the shell, a returned command, or an explicit effect type. Do not log from a supposedly pure helper. |

## Recursion, laziness, and honesty

| ID | Rule | Use when | Avoid when |
| --- | --- | --- | --- |
| FD-7 | Recursion | The data is recursive (trees, nested docs) or the language TCO-folds naturally | A simple loop is clearer and the team does not think in folds. Never recurse unbounded on user-sized lists without a plan. |
| FD-8 | Laziness | You need a pipeline over a large or infinite sequence without realizing it all | Debuggability dies (hidden evaluation, surprise I/O when a stream is forced). Prefer eager when the set is small. |

Name intermediate steps (`G19`) even in a pipeline. Clever point-free chains that hide types or effects fail FD-2.

## SOLID in function terms

| SOLID | Functional reading | Flag when |
| --- | --- | --- |
| SP-1 SRP | One function, one reason to change; one closure does not mix policy and I/O | A lambda that validates, logs, and writes |
| SP-2 OCP | Add a function or pass a new strategy function; do not edit a closed switch of cases | New case requires opening a pure core module that should have dispatched |
| SP-3 LSP | A function that claims to accept type T works for every T, including “empty” / special cases | Special cases throw or are rejected after the type already admitted them |
| SP-4 ISP | Small signatures; do not pass a fat record so the callee can pick two fields | Function takes a whole service object and calls one method |
| SP-5 DIP | Depend on function types / ports; inject effectful collaborators | Pure module imports a concrete client, clock, or DB |

Command vs function: prefer a function. Reach for Replace Function with Command only when the call needs identity, queueing, or undo.

## Data as history — FD-9

When the domain is “what happened” more than “what is”:

- Prefer appending facts (events, audit records) over mutating a row into amnesia.
- Derive current state with a pure fold over history (Combine Functions into Transform).
- Do not build an event-sourced system by default — use this shape when audit, time-travel, or conflict reconstruction is a real requirement. Otherwise an immutable value plus replacement is enough.

## Hybrid OO

Use this file in Java, C#, TypeScript, Kotlin, Swift, Ruby, etc. The language is not an excuse to mutate by default.

- **Value objects:** equality by value, no identity, no setters. Money, ranges, IDs, dates.
- **`final` / `readonly` / `const`:** fields and references. If a framework needs setters, keep them at the edge (entities, DTO mappers), not in policy.
- **No hidden mutation:** a method named like a query must not change fields (`N7`, Separate Query from Modifier).
- **Entities vs values:** if identity and lifecycle are real, isolate mutation to the entity’s explicit command methods; keep calculations on values.
- **Collections:** do not return a live internal list. Encapsulate Collection, or return an immutable copy.
- **Concurrency:** immutable values plus explicit shells beat synchronized mutable graphs. Flag shared mutable state even if you do not design the concurrency (`RS-5`, `RS-6`).

## Review notes

- Cite `FD-n` (and `RS-6` / `SP-5` when they coincide).
- **Must fix:** hidden mutation in a calculation; I/O inside domain rules you are changing; tests that cannot run without effects because the core is not pure.
- **Should fix:** in-place updates that could be replacement; loops that should be pipelines in this language.
- **Nit:** an entity with a setter you are not touching; laziness style in a hot path you have not measured.
