# Refactoring catalog

Named moves from Martin Fowler, *Refactoring* 2nd edition, grouped as on [refactoring.com/catalog](https://refactoring.com/catalog/). Primary name is 2nd edition. Aliases are 1st-edition or common IDE names. Mechanics are one line — load this file when choosing a move, not on every review.

Apply one move at a time; tests green between steps. See [refactoring.md](refactoring.md) for when and why.

## Basic

| Name | Aliases | Mechanics |
| --- | --- | --- |
| Extract Function | Extract Method | Move a fragment into a new function named for its intent; call it from the original. Inverse of Inline Function. |
| Inline Function | Inline Method | Replace a call with the function’s body; remove the function if unused. |
| Extract Variable | Introduce Explaining Variable | Put an expression’s result in a named variable that states its meaning. Inverse of Inline Variable. |
| Inline Variable | Inline Temp | Replace a temp that adds no meaning with its expression. |
| Change Function Declaration | Rename Method, Rename Function, Add Parameter, Remove Parameter, Change Signature | Change a function’s name, parameters, or both; update all callers. |
| Encapsulate Variable | Encapsulate Field, Self-Encapsulate Field | Route reads/writes through functions; keep the storage private. |
| Rename Variable | | Change the name to match current meaning; update all references in scope. |
| Introduce Parameter Object | | Replace a clump of parameters with a single object (or record) that names the concept. |
| Combine Functions into Class | | Move a group of functions that share data into a class that holds that data. |
| Combine Functions into Transform | | Merge functions that derive related results from the same source into one transform that returns a record. |
| Split Phase | | Split sequential work into phases with an explicit intermediate data structure between them. |

## Encapsulation

| Name | Aliases | Mechanics |
| --- | --- | --- |
| Encapsulate Record | Replace Record with Data Class | Wrap a record/hash in a type; hide direct field access behind a clear API. |
| Encapsulate Collection | | Return a copy or unmodifiable view; mutate only through add/remove methods on the owner. |
| Replace Primitive with Object | Replace Data Value with Object, Replace Type Code with Class | Replace a primitive that carries domain meaning with a small type that owns its operations. |
| Replace Temp with Query | | Extract the temp’s expression into a function; call it instead of storing. |
| Extract Class | | Move a cohesive subset of fields and methods into a new class; delegate from the old. |
| Inline Class | | Move all of a class’s features into another and delete the empty class. |
| Hide Delegate | | Add a delegating method on the server so clients stop talking to the delegate. |
| Remove Middle Man | | Drop delegating methods; let clients call the delegate directly. |
| Substitute Algorithm | | Replace a body with a clearer algorithm that preserves results. |

## Moving features

| Name | Aliases | Mechanics |
| --- | --- | --- |
| Move Function | Move Method | Move a function to the type that uses it most; update callers. |
| Move Field | | Move a field to the type that needs it; adjust references. |
| Move Statements into Function | | Slide a fragment that belongs with a callee into that function. |
| Move Statements to Callers | | Slide a fragment out of a function into each caller that needs it, then simplify. |
| Replace Inline Code with Function Call | | Replace a fragment with a call to an existing function that already does it. |
| Slide Statements | | Move adjacent, related statements together (or apart) without changing order-sensitive results. |
| Split Loop | | Replace one loop that does two things with two loops that each do one. |
| Replace Loop with Pipeline | | Replace a loop with map / filter / reduce (or language equivalent) that names each step. |
| Remove Dead Code | | Delete code that is never executed or never referenced. |

## Organizing data

| Name | Aliases | Mechanics |
| --- | --- | --- |
| Split Variable | Split Temporary Variable, Remove Assignments to Parameters | Each responsibility gets its own variable; do not reuse a temp or parameter for a second meaning. |
| Rename Field | | Rename a field to match its current meaning. |
| Replace Derived Variable with Query | | Drop a field that can be computed; replace reads with a function. |
| Change Reference to Value | | Make the object immutable and compare by value; replace shared mutation with replacement. |
| Change Value to Reference | | Replace many equal values with one shared reference object when identity matters. |

## Simplifying conditional logic

| Name | Aliases | Mechanics |
| --- | --- | --- |
| Decompose Conditional | | Extract the condition and each branch into named functions. |
| Consolidate Conditional Expression | Consolidate Duplicate Conditional Fragments | Combine conditions that lead to the same result (or lift duplicated branch fragments). |
| Replace Nested Conditional with Guard Clauses | | Handle edge/fail cases first with early returns; keep the happy path unindented. |
| Replace Conditional with Polymorphism | Replace Type Code with State/Strategy | Move each branch onto a type that represents that case; dispatch through a common interface. |
| Introduce Special Case | Introduce Null Object | Create a special-case type (often for null / unknown) that absorbs the checks. |
| Introduce Assertion | | Make an assumed condition explicit and fail fast if it is false. |

## Refactoring APIs

| Name | Aliases | Mechanics |
| --- | --- | --- |
| Separate Query from Modifier | | Split a function that both answers and changes state into a query and a modifier. |
| Parameterize Function | Parameterize Method | Merge similar functions by adding a parameter for the variation. |
| Remove Flag Argument | | Replace a boolean/enum that selects behavior with a separate function per path. |
| Preserve Whole Object | | Pass the object instead of several of its fields. |
| Replace Parameter with Query | | Drop a parameter the callee can obtain itself; call a query instead. |
| Replace Query with Parameter | | Pass the value in so the callee does not reach out for it. |
| Remove Setting Method | | Drop a setter; set the field only at construction (or not at all). |
| Replace Constructor with Factory Function | Replace Constructor with Factory Method | Replace a public constructor with a named factory that can vary type or fail clearly. |
| Replace Function with Command | | Turn a complex function into an object that holds the call’s state and execution. |
| Replace Command with Function | | Flatten a command object back into a function when the object adds no value. |

## Dealing with inheritance

| Name | Aliases | Mechanics |
| --- | --- | --- |
| Pull Up Method | | Move identical methods from subclasses into the superclass. |
| Pull Up Field | | Move identical fields from subclasses into the superclass. |
| Pull Up Constructor Body | | Move common constructor steps into a superclass constructor (or init) called by subclasses. |
| Push Down Method | | Move a method used by only some subclasses into those subclasses. |
| Push Down Field | | Move a field used by only some subclasses into those subclasses. |
| Replace Type Code with Subclasses | Extract Subclass | Create a subclass per type-code value; replace the code with the type. |
| Remove Subclass | Replace Subclass with Fields | Fold a subclass that adds little into the superclass (often via a field). |
| Extract Superclass | | Create a superclass for duplicated features of two similar classes. |
| Collapse Hierarchy | | Merge a superclass and subclass that are not different enough. |
| Replace Subclass with Delegate | | Move subclass variation into a delegate; the former hierarchy uses composition. |
| Replace Superclass with Delegate | Replace Inheritance with Delegation | Replace “is-a” inheritance with a field that the former subclass delegates to. |

## Notes

- Prefer 2nd-edition names in reviews and summaries. Mention an alias only if the team’s IDE or codebase already uses it.
- 1st-edition “Replace Magic Number with Symbolic Constant” is still worth doing; cite `G25` from [smells.md](smells.md).
- Form Template Method is a design pattern ([solid.md](solid.md)); reach it by Extract Function + Pull Up Method, not as a first move.
