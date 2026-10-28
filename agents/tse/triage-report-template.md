# Triage: <short title>

**Reported:** <date>
**Reporter:** <who>
**Surface:** <feature / screen / area>
**Headline:** `<n BUG, n SPEC CONFLICT, n FEATURE, n GAP>`
**Specs read:** `<paths>`

## Report as filed

> <the original words, verbatim>

**Claims extracted:**

1. <checkable assertion>
2. <checkable assertion>

Separate the reporter's *explanation* from the *symptom*. Note here where the
stated explanation turned out not to hold — the symptom may still be real.

---

## TSE-1 — <one-line claim>

**Verdict:** BUG | SPEC CONFLICT | FEATURE | GAP
**Route:** clean-code | product-manager | ux-designer | none

| | |
| --- | --- |
| **Requirement** | `<FR-ID>` — [`<file>`](<file>#L<n>) — P0/P1/P2, shipped/deferred/cut/forbidden |
| **Quote** | "<short verbatim quote from the spec>" |
| **Code** | [`<path>:<line>`](<path>#L<line>) |
| **Actual behavior** | <what the code does> |
| **Conflicting spec** | *(SPEC CONFLICT only)* `<file:line>` — "<quote>" |
| **Invariant in play** | *(when relevant)* `<AD-ID>` — <file> |

**Why this verdict:** <1–2 sentences tying the two citations together.>

**For the receiving agent:**

- *BUG* — the requirement to satisfy, the states that must expose the behavior,
  and what must **not** change. Name the tests that should now exist.
- *SPEC CONFLICT* — which two documents disagree, and the decision required
  before any code moves.
- *FEATURE* — where this was cut and the stated reason; which invariants or
  deferred decisions a PRD would have to reopen; parent PRD path if this is a
  delta.

---

## TSE-2 — <one-line claim>

<repeat>

---

## Not defects

Claims that resolved as GAP, with the correct mental model, so the same report
does not come back. Keep these — a recurring GAP is itself a UX signal.

| Claim | How it actually works | Where the control lives |
| --- | --- | --- |
| <claim> | <explanation> | `<path:line>` / <screen + state> |

## Out of scope for this triage

<Anything noticed in passing that is not part of this report. Do not route it.>
