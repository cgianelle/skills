# Architectural thinking

Decision-forcing notes for the architect. Apply these; do not paste them into the architecture document.

## What counts as architecture

Ralph Johnson (via Fowler): architecture is the shared understanding expert developers have of the system design — "the important stuff. Whatever that is." There is no objective "fundamental" layer. The work is deciding what is important, then keeping those elements in good condition.

Two tests, both required:

1. If two implementers built the next slice independently, could they choose incompatibly in a way that matters?
2. Would getting that call wrong be expensive to reverse?

If either fails, defer. Architecture is not a document; the document records what must stay consistent.

## Internal quality is not a tradeoff against speed

Users do not perceive architecture directly. They perceive features arriving slowly and with more defects. That lag is usually cruft — structure that impedes understanding.

High internal quality buys faster delivery. Experienced developers reckon the payoff in weeks, not months. Sacrificing structure for speed only wins before cruft compounds, and people underestimate how fast that happens.

Use this to justify real structure. Do not use it to license gold-plating. A speculative context split is still a speculative context split.

## Boundaries are social constructions

An "application" is not a technical primitive. It is:

- a body of code developers treat as one unit
- a capability customers treat as one unit
- an initiative one budget funds

Draw seams around teams, ownership, and money — not only around nouns. At enterprise altitude, centralize only what is worth the coordination cost. Prefer local decisions; coordinate the few things that actually hurt when they drift.

## Evolutionary and sacrificial architecture

A good architecture supports its own evolution and is intertwined with programming. Prefer designs that stay cheap to change.

When a part is meant to be thrown away, say so. Name it sacrificial, name what it protects (a learning goal, a deadline, an unknown), and name the replacement trigger. Silent throwaways become accidental permanence.

## Ubiquitous language

To build software for a complex domain, embed domain terminology in the software itself. The language is not a glossary on paper; it lives in names, events, and APIs, and it evolves with the product.

- One term per concept, shared by PRD, UX, and code.
- Inherit PRD wording verbatim where it exists. A PRD term the code (or the architecture) silently renames is a defect. Flag the conflict; do not invent a synonym.
- The same word in two bounded contexts is two models. Unifying them is usually the mistake.

## Bounded contexts are the top-level decomposition

Contexts before components. A bounded context is a boundary inside which a particular model and language are consistent. Outside it, the same words may mean something else.

At system altitude, draw the contexts first, then decide how they integrate, then decide internals. At feature altitude, inherit existing contexts rather than inventing new ones unless the feature creates a new language seam.

## Distillation

Label each subdomain:

| Kind | Spend | Typical fate |
|------|-------|--------------|
| **Core** | Design effort, distinctive model | The part the business would not outsource |
| **Supporting** | Keep dull and explicit | Necessary, not differentiating |
| **Generic** | Buy, adopt, or copy | Auth, billing adapters, file storage, … |

If everything is "core," nothing is. Put energy where a wrong model is expensive.

## Context map

Every edge between contexts gets a **dependency direction** (upstream / downstream) and a **named integration pattern**. Unnamed edges are undeclared coupling.

| Pattern | Use when |
|---------|----------|
| Shared Kernel | Both sides truly share a small model and will co-evolve it |
| Customer/Supplier | Downstream can negotiate; upstream accounts for it |
| Conformist | Downstream accepts upstream's model as-is |
| Anticorruption Layer | Downstream translates; protects its model from a foreign one |
| Separate Ways | Integration costs more than duplication; no shared model |
| Open Host Service | Upstream offers a protocol several consumers use |
| Published Language | The shared document/schema *is* the contract |
| Big Ball of Mud | Honest label for a tangle you are not splitting yet |

## Aggregates and domain events (tactical, light)

Stop here. Entities, value objects, repositories, and domain services stay with `clean-code` unless they are the consistency rule itself.

- An **aggregate** is a consistency boundary: one transaction, one invariant set, referenced from outside by identity only.
- Cross-aggregate consistency is eventual. The architecture must say how (event, process, outbox, human follow-up) — or that it is in-process and same-transaction, which means they were not separate aggregates.
- **Domain events** are past-tense names from the ubiquitous language. They are the contract between contexts, not log lines.

## When this does not apply

Rule of Three still governs. Do not introduce a shared model, a new context, or an anticorruption layer until three concrete cases need it.

- Feature / slice altitude: inherit contexts; skip Domain Landscape if there is one obvious context.
- Two-screen CRUD, a framework upgrade, a pin bump: language table of the terms you must not rename is enough; no five-context map.
- Greenfield with a thin domain: one context, named, is a decision. Splitting it is the thing to justify.

## Anti-patterns to refuse

- DDD-shaped folders (`/domain`, `/aggregates`) with no model and no language
- Anemic domain model presented as "the domain layer"
- One bounded context per service by reflex
- A language table nobody uses, or terms that contradict the PRD
- Speculative context splits "for later scale"
- Treating Big Ball of Mud as shame instead of an honest current state
- Central architecture approval for every local decision

## Interview prompts (coaching path)

Ask in batches of 2–4. Skip any already answered.

- What do the experts call this? Where does one word mean two things?
- Which part would the business never outsource? What would they buy?
- If two teams shipped the next slice apart, what would they get wrong?
- Who is upstream of whom, and can the downstream negotiate?
- What is allowed to be wrong together in one transaction? What must wait?
- What do we expect to throw away, and what would trigger replacing it?

## Self-review additions

On top of "every AD prevents a real divergence":

- Language matches the PRD's terms, or conflicts are explicit
- Every in-scope FR maps to an owning context (or the altitude inherited one context)
- Every context edge has a named pattern and a direction
- Core / supporting / generic is labeled, or the section is honestly omitted
- Consistency boundaries and cross-boundary "how" are stated
- Sacrificial parts have a replacement trigger
- Nothing here is ceremony for a slice that did not need it

## Sources

- [Software Architecture Guide](https://martinfowler.com/architecture/) — Johnson via Fowler on what architecture is; internal quality; applications as social constructions; enterprise coordination
- [Domain-Driven Design (bliki)](https://martinfowler.com/bliki/DomainDrivenDesign.html) — ubiquitous language in software; strategic design; aggregates
- [domainlanguage.com/ddd](https://www.domainlanguage.com/ddd/) — Evans's DDD: bounded contexts, context mapping, distillation
