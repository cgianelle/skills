# Refactoring

Distilled from Martin Fowler, *Refactoring* (2nd edition) and [refactoring.com](https://refactoring.com/). Not a verbatim reproduction. Cite smells as `RS-n`. Name every proposed or applied move by its catalog name; mechanics live in [refactoring-catalog.md](refactoring-catalog.md).

## Two hats

Never wear both at once:

| Hat | Allowed | Forbidden |
| --- | --- | --- |
| **Feature** | New behavior, bug fixes | Restructuring “while we’re here” |
| **Refactoring** | Behavior-preserving structure change | New features, bug fixes, drive-by formatting of unrelated files |

Switch hats between green test runs. If a bug appears during a refactor, revert the step, put the feature hat on to fix (with a test), then resume refactoring.

## When to refactor

| Moment | Cue | Bound |
| --- | --- | --- |
| **Preparatory** | The next change is hard in the current shape | Make the change easy, then make the easy change. Stop when the feature fits. |
| **Comprehension** | You just understood a name, boundary, or algorithm | Rename / extract immediately so the next reader does not re-derive it. |
| **Litter-pickup** | You are already in the file and see a small mess | Boy Scout: tiny, local, same-diff. No unrelated modules. |
| **Planned / opportunistic** | A smell blocks delivery, or a sequence of small moves will pay down a hotspot | Prefer a string of opportunistic steps over a rewrite. Long-term campaigns stay a series of green, committable slices. |

Do not refactor on red tests, on a deadline-crunched unrelated path, or as speculative cleanup of code you are not changing.

## Small-step discipline

1. Tests green before the first move. If they are not, stop — refactoring has no safety net.
2. One catalog move (or a trivially smaller edit) at a time.
3. Run tests after each step. Stay green.
4. Red after a step → revert that step; do not “fix forward” by adding behavior.
5. Commit (or at least checkpoint) per step when the environment allows. Each commit should be a named refactoring, not a mixed hat.
6. Prefer the compiler / type checker / automated refactoring as the first net, tests as the second.

## Smell catalog (Ch. 3)

Each row: rule — when to flag — first moves to consider. Pair with [smells.md](smells.md) IDs when they overlap (`G5` duplication, `G14` feature envy, `G36` message chains, …).

| ID | Smell | Flag when | First moves |
| --- | --- | --- | --- |
| RS-1 | Mysterious Name | Name does not reveal role, or requires a comment to decode | Rename Function / Variable / Field; Change Function Declaration |
| RS-2 | Duplicated Code | Same expression or logic in two or more places | Extract Function; Slide Statements; Pull Up Method |
| RS-3 | Long Function | Function mixes abstraction levels, or you cannot see its purpose at a glance | Extract Function; Replace Temp with Query; Introduce Parameter Object; Decompose Conditional; Replace Conditional with Polymorphism; Split Loop; Replace Loop with Pipeline |
| RS-4 | Long Parameter List | Three or more parameters, especially a recurring group | Replace Parameter with Query; Introduce Parameter Object; Preserve Whole Object; Remove Flag Argument |
| RS-5 | Global Data | Mutable data reachable from anywhere (globals, singletons-as-scope, public statics) | Encapsulate Variable; then shrink the visible set |
| RS-6 | Mutable Data | State changes from multiple places, or mutation is hidden by a query-like name | Encapsulate Variable; Split Variable; Separate Query from Modifier; Remove Setting Method; Replace Derived Variable with Query; Combine Functions into Transform; Change Reference to Value |
| RS-7 | Divergent Change | One module edited for several unrelated reasons | Split Phase; Move Function; Extract Function; Extract Class |
| RS-8 | Shotgun Surgery | One change requires many tiny edits across modules | Move Function; Move Field; Combine Functions into Class; Inline Function; Inline Class |
| RS-9 | Feature Envy | A function uses another type’s data more than its own | Move Function; Extract Function then Move Function |
| RS-10 | Data Clumps | The same group of fields / parameters always travel together | Extract Class; Introduce Parameter Object; Preserve Whole Object |
| RS-11 | Primitive Obsession | Primitive stands in for a concept (money, range, type code, phone) | Replace Primitive with Object; Replace Type Code with Subclasses; Replace Conditional with Polymorphism; Introduce Parameter Object |
| RS-12 | Repeated Switches | The same type-switch / if-chain appears in more than one place | Replace Conditional with Polymorphism |
| RS-13 | Loops | A loop is a pipeline (map / filter / reduce) written by hand, and the language has one | Replace Loop with Pipeline |
| RS-14 | Lazy Element | Class, function, or subclass that does too little to justify its existence | Inline Function; Inline Class; Collapse Hierarchy |
| RS-15 | Speculative Generality | Hooks, parameters, or types added “for later” with one (or zero) real uses | Collapse Hierarchy; Inline Function / Class; Change Function Declaration; Remove Dead Code |
| RS-16 | Temporary Field | A field is set only in some cases and unused otherwise | Extract Class; Introduce Special Case |
| RS-17 | Message Chains | `a.getB().getC().do()` — callers know a graph they should not | Hide Delegate; Extract Function; Move Function |
| RS-18 | Middle Man | Most of a type’s methods only delegate to someone else | Remove Middle Man; Inline Function; Replace Superclass / Subclass with Delegate |
| RS-19 | Insider Trading | Two modules rummage in each other’s internals | Move Function / Field; Hide Delegate; put the shared secret in a third module |
| RS-20 | Large Class | Too many fields or methods; low cohesion; many reasons to change | Extract Class; Extract Superclass; Replace Type Code with Subclasses; Extract Function |
| RS-21 | Alternative Classes with Different Interfaces | Two types do the same job with different method names | Change Function Declaration; Move Function |
| RS-22 | Data Class | Fields plus getters/setters and almost no behavior | Encapsulate Record; Encapsulate Collection; Move Function onto it; Remove Setting Method |
| RS-23 | Refused Bequest | Subclass ignores or fights inherited interface / implementation | Push Down Method / Field; Replace Subclass with Delegate; Replace Superclass with Delegate |
| RS-24 | Comments | Comment exists because the code will not speak | Extract Function; Change Function Declaration; Introduce Assertion. Keep comments that warn of consequences. |

## Review notes

- Cite `RS-n` plus the catalog name you would apply. Do not invent new names.
- **Must fix:** smell that will make the current change wrong, unsafe, or untestable.
- **Should fix:** smell on the path you are already on.
- **Nit:** smell off the path, or speculative generality you are not paid to unwind.
