# TDD and craftsmanship

Distilled from Robert C. Martin, *Clean Craftsmanship* (and the TDD discipline in *Clean Code* / *Agile Software Development*). Not a verbatim reproduction. Cite as `TD-n`. Pair with F.I.R.S.T. and `T1`–`T9` in [smells.md](smells.md).

## Default: test-first

If the repo has a runnable test command, follow the three laws. The only escape hatch is **no runnable test setup** — state that in the return summary and do not invent a framework unless asked.

## Three laws (nano-cycle) — TD-1

Seconds, not minutes:

1. Do not write production code until a unit test is failing.
2. Do not write more of a test than is sufficient to fail (a compile / type error counts as failure).
3. Do not write more production code than is sufficient to pass the currently failing test.

Then refactor under a green bar ([refactoring.md](refactoring.md)). Repeat.

## Cycle scales — TD-2

| Cycle | Time | What happens |
| --- | --- | --- |
| **Nano** | seconds | The three laws: one failing assertion, one sliver of production code. |
| **Micro** | minutes | Red → green → refactor. Refactor both test and production on green. Do not skip the third step. |
| **Milli** | tens of minutes | Specific tests, generic production (see TD-3). When a new test forces an ugly branch, generalize the design instead of stacking special cases. |
| **Primary** | story / session | Acceptance behavior in mind; unit tests drive the internals; keep the suite passing so the work is always one step from shippable. |

If you cannot name which cycle you are in, you are likely mixing hats or writing production without a failing test.

## Transformation Priority Premise — TD-3

When a failing test can be made to pass in more than one way, prefer the simpler transformation (higher on this list):

1. `{}` → nil / null / empty
2. nil → constant
3. constant → more constants (or a slightly richer constant)
4. constant → variable / scalar
5. unconditional statement → more statements
6. unconditional → `if`
7. scalar → array / collection
8. array → richer container
9. statement → tail recursion (or the language’s equivalent fold)
10. `if` → `while` / loop
11. expression → function
12. variable → assignment (mutation) — last, not first

Skip down the list only when a higher transformation cannot express the new behavior. This is how production code grows generic while tests stay specific.

## Test doubles — TD-4

Use the fewest and weakest double that still isolates the subject. Prefer real values, fakes, and stubs over mocks.

| Double | Role | Legitimate when |
| --- | --- | --- |
| **Dummy** | Passed but never read | Filling a required argument you do not care about. |
| **Stub** | Returns canned data | The subject needs a collaborator answer, not an interaction script. |
| **Spy** | Records calls for later assertion | You must confirm an outgoing call (usually at a true boundary). |
| **Mock** | Pre-programmed expectations; fails on mismatch | Rare. Protocol with an external system where call order/args *are* the spec. |
| **Fake** | Working simplified implementation | In-memory DB, clock, filesystem — replace infrastructure, not domain logic. |

Do not mock types you own that could be faked or constructed. Do not mock through three layers. Learning tests (real library, small suite) beat a mock of a third-party API.

## Fragile tests — TD-5

Flag and avoid:

- Tests that break when you rename a private, change call order, or extract a function — they froze an implementation.
- One test that asserts many unrelated facts (splits into one concept per test).
- Exact wall-clock, exact log strings, or full-document snapshots unless that *is* the behavior.
- Shared mutable fixtures that couple tests (`T9` / Independent).

**Specific vs generic:** a test should pin one behavior (specific). Production code should handle the family of cases (generic). When a test knows too much about *how*, delete the interaction assertions and assert the outcome.

## Acceptance tests — TD-6

- Describe a story from the outside: given / when / then in domain language, not UI-driver trivia.
- They do not replace unit tests. They constrain the primary cycle; unit tests drive the nano/micro cycles.
- Write or name the acceptance behavior before the first unit test of the story when the build package or user supplies it.
- Keep them as slow as they must be and as few as they can be. Fast unit tests remain the default net.

## Professional standards as behaviors — TD-7

These are operating rules, not slogans:

| Standard | Agent behavior |
| --- | --- |
| Never ship known broken | Do not return with a red suite. Do not comment out failing tests to get green. |
| Continuous readiness | After every micro-cycle the tree could ship that slice. No “I’ll clean it at the end.” |
| QA should find nothing | Do not defer obvious craft, boundary, or missing-test defects to a later reviewer. Cover a bug with a failing test before fixing it (`T6`). |
| Stable productivity | Do not go faster by skipping tests, doubles at the wrong layer, or mixed-hat diffs. Same pace, always. |
| Tests prove behavior | A test whose name and assertion a teammate cannot explain is not done (`G21`). |
| Boy Scout | Leave the files you touched cleaner — locally, not as a rewrite. |

## Write-mode checklist

1. Identify the test command. Missing → state the escape hatch and stop using this file for process.
2. Red: smallest failing test, one concept, name that states the behavior.
3. Green: least production code that passes (TD-3).
4. Refactor: names, duplication, structure; tests stay green.
5. Repeat until the requested behavior is covered at boundaries (`T5`).
6. Return: what was built, tests added, test command and result.
