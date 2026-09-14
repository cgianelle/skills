# SOLID and pattern-level design

Distilled from Robert C. Martin, *Agile Software Development: Principles, Patterns, and Practices* (and the SOLID chapters of *Clean Architecture* / *Clean Craftsmanship*). Not a verbatim reproduction. Cite principles as `SP-n`. Patterns are tools to resolve a principle, not décor.

**Scope:** class, module, and function. Component/package principles (REP, CCP, CRP, ADP, SDP, SAP) and system boundaries → `architect`.

**Rule of Three:** do not introduce a shared abstraction or pattern until three concrete cases need it. Two copies may be duplication you extract; one copy plus a hypothetical future is speculative generality (`RS-15`).

## Principles

| ID | Principle | Meaning | Flag when | First fix |
| --- | --- | --- | --- | --- |
| SP-1 | SRP — Single Responsibility | A module has one reason to change: one actor. | Class or file edited for unrelated stakeholders; mixed I/O, policy, and formatting; “and” in the type name. | Extract Class / Extract Function; split along the actor, not along a technical layer. |
| SP-2 | OCP — Open-Closed | Open for extension, closed for modification. | Adding a case means editing a switch / if-chain in a closed module; the same edit in many places (`RS-12`). | Polymorphism: Strategy, Template Method, or a dispatch table at a stable seam. |
| SP-3 | LSP — Liskov Substitution | A subtype is usable wherever its base/type is promised. | `if (x instanceof …)`, subclass throws on inherited methods, empty overrides, subtype tightens preconditions or weakens postconditions. | Honor the contract or stop inheriting: Push Down, Replace Subclass / Superclass with Delegate. |
| SP-4 | ISP — Interface Segregation | Clients must not depend on methods they do not use. | Fat interface; implementors with no-ops or `UnsupportedOperation`; a change to an unused method forces recompiles/retests of bystanders. | Split the interface; give each client the methods it calls. |
| SP-5 | DIP — Dependency Inversion | High-level policy depends on abstractions, not on details. Details depend on the same abstractions. | Domain module imports SQL, HTTP, or a framework type; `new` of a volatile concrete buried in policy; tests cannot run without the real I/O. | Introduce an interface owned by the policy; inject the concrete at the edge. Factory if creation itself is volatile. |

SRP is about *who* wants the change, not “does one thing” in the mechanical sense. A function still does one thing ([smells.md](smells.md) `G30`); a class still has one actor.

## Resolving patterns

Use when an `SP-n` violation is real and the third case has arrived. Prefer the simplest move in [refactoring-catalog.md](refactoring-catalog.md) that removes the smell; reach for a named pattern only if the structure will still be there tomorrow.

| Pattern | Helps | Mechanics (one line) | Avoid when |
| --- | --- | --- | --- |
| **Strategy** | SP-2, SP-1 | Isolate a varying algorithm behind an interface; inject it. | A single algorithm with no second variant. |
| **Template Method** | SP-2 | Invariant steps in a base; varying steps as overridable hooks. | The hierarchy exists only to share a few lines — prefer Strategy or a function parameter. |
| **Command** | SP-1, SP-5 | An action as an object (or function value) so callers do not know the work. | A plain function would do; see Replace Command with Function. |
| **State** | SP-2, RS-12 | Each state is a type; transitions replace a mode flag / switch. | Two states that will never grow — a boolean may be honest. |
| **Visitor** | SP-2 | Add an operation across a stable set of types without editing those types. | The type set is what changes, not the operations — then add a method on the types. |
| **Factory / Abstract Factory** | SP-5 | Policy asks a factory for an abstraction; construction of concretes lives at the edge. | The concrete is stable and local — `new` is fine. |
| **Adapter** | SP-5, SP-4 | Wrap a foreign API in the interface the policy already speaks. | You control both sides and can change the real type. |
| **Proxy** | SP-5 | Stand-in with the same interface (lazy, remote, access). | Indirection with no extra policy (then it is a Middle Man, `RS-18`). |
| **Observer** | SP-2, SP-1 | Publisher notifies subscribers without knowing who they are. | Two modules that should just call each other; event soup hides a missing type. |
| **Null Object / Special Case** | SP-3 | A subtype that does the “missing” behavior instead of `null` checks. | Absence is exceptional and should abort — use an error type. |
| **Decorator** | SP-2, SP-4 | Wrap an interface to add behavior without changing the core. | You need to change the core contract itself. |

Command, Strategy, and a higher-order function are the same idea at different syntaxes. In a functional or hybrid codebase, prefer passing a function before introducing a strategy type.

## Review notes

- Cite `SP-n` and, if you recommend a pattern, name it. Do not prescribe a pattern when Extract Class or Move Function is enough.
- **Must fix:** DIP/LSP breaks that make tests or substitution lie; SRP mixes that will force conflicting edits in this change.
- **Should fix:** OCP/ISP friction on the path you are on.
- **Nit:** a pattern you might want someday (`RS-15`).
