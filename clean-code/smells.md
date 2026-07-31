# Smells and Heuristics

Condensed from Clean Code Ch. 17. Use IDs in reviews (`G30`, `N1`, …). Each line: rule — when to flag.

## Comments

| ID | Rule | Flag when |
| --- | --- | --- |
| C1 | Inappropriate information | Comment holds change history, authorship, or other non-local metadata |
| C2 | Obsolete comment | Comment no longer matches the code |
| C3 | Redundant comment | Comment restates what the code already says clearly |
| C4 | Poorly written comment | Vague, rambling, or hard to understand |
| C5 | Commented-out code | Dead code left commented instead of deleted (VCS exists) |

## Environment

| ID | Rule | Flag when |
| --- | --- | --- |
| E1 | Build requires more than one step | Build/setup is multi-step tribal knowledge |
| E2 | Tests require more than one step | Running tests needs manual ceremony beyond one command |

## Functions

| ID | Rule | Flag when |
| --- | --- | --- |
| F1 | Too many arguments | More than ~2–3 parameters; especially long positional lists |
| F2 | Output arguments | Function mutates an argument as its “result” channel |
| F3 | Flag arguments | Boolean (or enum) selects behavior — usually two functions |
| F4 | Dead function | Never called; remove |

## Names

| ID | Rule | Flag when |
| --- | --- | --- |
| N1 | Choose descriptive names | Name does not reveal intent |
| N2 | Appropriate abstraction level | Name encodes lower/higher detail than the type’s role |
| N3 | Standard nomenclature | Ignores project/domain/language conventions without reason |
| N4 | Unambiguous names | Name could mean several things in context |
| N5 | Long names for long scopes | Tiny cryptic names used across large scope |
| N6 | Avoid encodings | Hungarian, type prefixes, member `m_` noise |
| N7 | Describe side effects | Name hides mutation, I/O, or other side effects |

## Tests

| ID | Rule | Flag when |
| --- | --- | --- |
| T1 | Insufficient tests | Behavior changed/added without covering tests |
| T2 | Use a coverage tool | Untested paths unknown; coverage not consulted when relevant |
| T3 | Don't skip trivial tests | “Obvious” cases undocumented and untested |
| T4 | Ignored test = ambiguity | `@Ignore` / `skip` without explaining the open question |
| T5 | Test boundary conditions | Edges, empty, null/optional, overflow, off-by-one missing |
| T6 | Exhaustively test near bugs | Fix landed without nearby regression coverage |
| T7 | Failure patterns are revealing | Repeated similar failures not investigated as a design smell |
| T8 | Coverage patterns are revealing | Hotspots of low coverage on critical logic |
| T9 | Tests should be fast | Suite slow enough that people avoid running it |

## General

| ID | Rule | Flag when |
| --- | --- | --- |
| G1 | Multiple languages in one source file | Unrelated languages/DSLs mixed without clear need |
| G2 | Obvious behavior unimplemented | API/name promises behavior that is missing |
| G3 | Incorrect behavior at boundaries | Edge cases wrong or unhandled |
| G4 | Overridden safeties | Safety checks, warnings, or locks disabled |
| G5 | Duplication | Same logic/knowledge repeated |
| G6 | Wrong level of abstraction | Details leak upward or policy sinks downward |
| G7 | Base depends on derivatives | Base class knows about subclasses |
| G8 | Too much information | Overexposed APIs; large interfaces; leaky internals |
| G9 | Dead code | Unreachable or unused code paths |
| G10 | Vertical separation | Related functions/vars far apart vertically |
| G11 | Inconsistency | Similar things done differently without cause |
| G12 | Clutter | Noise variables, dead imports, unused params |
| G13 | Artificial coupling | Unrelated things forced together |
| G14 | Feature envy | Method uses another type’s data more than its own |
| G15 | Selector arguments | Arguments only choose which path to run (see F3) |
| G16 | Obscured intent | Algorithm or purpose hard to see |
| G17 | Misplaced responsibility | Code lives in the wrong module/type |
| G18 | Inappropriate static | Static used where instance polymorphism/state fits |
| G19 | Use explanatory variables | Complex expressions without naming intermediate meaning |
| G20 | Function names say what they do | Name mismatches behavior |
| G21 | Understand the algorithm | Working code no one can explain |
| G22 | Make logical dependencies physical | Hidden coupling not expressed in structure/types |
| G23 | Prefer polymorphism to if/else or switch | Growing type-switch that should be polymorphic dispatch |
| G24 | Follow standard conventions | Local style/idioms ignored |
| G25 | Replace magic numbers with named constants | Bare literals with meaning |
| G26 | Be precise | Vague behavior, fuzzy APIs, sloppy conditions |
| G27 | Structure over convention | Relying on naming convention where types/structure work |
| G28 | Encapsulate conditionals | Complex boolean exposed inline instead of named predicate |
| G29 | Avoid negative conditionals | Hard-to-parse `!isNotReady`-style logic when positive form is clearer |
| G30 | Functions should do one thing | Multiple responsibilities or abstraction levels in one function |
| G31 | Hidden temporal couplings | Order-dependent calls with no API enforcement |
| G32 | Don't be arbitrary | Structure/ordering with no coherent reason |
| G33 | Encapsulate boundary conditions | Off-by-one / edge arithmetic scattered |
| G34 | One level of abstraction per function | Mixed high-level and low-level steps |
| G35 | Configurable data at high levels | Magic config buried deep instead of passed/owned high |
| G36 | Avoid transitive navigation | Long `a.b.c.d` chains (train wrecks) |

## Java / OO (optional)

Apply only when the language/ecosystem matches; otherwise skip or restate in local idiom.

| ID | Rule | Flag when |
| --- | --- | --- |
| J1 | Avoid long import lists via wildcards | Wildcard imports hide dependencies (or the reverse project rule is violated) |
| J2 | Don't inherit constants | Constants inherited to avoid qualification |
| J3 | Constants versus enums | Related constant groups that should be a typed enum/sum type |
