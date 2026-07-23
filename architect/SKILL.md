---
name: architect
description: Full architecture workflow from discovery through trade-offs, design, documentation, and validation. Produces a lean architecture document of invariants and decisions that keep implementation consistent. Use when the user explicitly invokes the architect skill to design system architecture, technical design, solution architecture, or an architecture spine.
disable-model-invocation: true
---

# Architect — Technical Architecture

## Overview

Drive architecture through a structured workflow: discover context and constraints, evaluate trade-offs, lock invariants, document decisions, then validate. Produce a lean architecture artifact — not a speculative encyclopedia — that keeps independently built work consistent.

## Ground Rules

- Discover first, write second. Never generate a full architecture from a one-line idea without asking questions (unless the user explicitly chooses the Fast path).
- Ask questions in small batches (2–4 at a time), not one giant questionnaire. Use the AskQuestion tool when available.
- Answer with trade-offs, not verdicts. For load-bearing calls, show realistic alternatives, lean one way, and let the user choose.
- Prefer boring technology and known starters over novel stacks unless the problem clearly demands otherwise.
- Rule of Three before abstraction: don't introduce a shared layer until three concrete cases need it.
- Developer productivity is architecture: optimize for clarity, change cost, and onboarding — not cleverness.
- Record decisions, not essays. Rationale lives in the conversation; the doc carries what binds and what it prevents.
- Distinguish facts from assumptions. Tag unverified claims as `[ASSUMPTION]` or list them under Open Questions.
- One test for what belongs in the architecture: if two teams built the next level independently, could they choose incompatibly in a way that matters? Fix it only when the answer is yes, the call is non-obvious, and it's a real trade-off. Otherwise defer it.
- Verify named technologies' current versions and fit on the web before binding them.
- Do not invent to fill gaps. Mark missing answers as open questions.

## Working Modes

Offer this choice before drafting unless the user already made it clear:

1. **Coaching path (default)** — elicit decisions together; push back when a call is thin.
2. **Fast path** — draft the full architecture with `[ASSUMPTION]` tags for the user to correct in review.

Unless the user asks for speed, coach; don't silently draft.

## Workflow

### Step 1: Understand the starting point

Check what already exists before interviewing:

- PRD, UX specs, notes, or prior architecture docs the user points to
- Workspace context: README, existing architecture, project structure, ADRs
- Brownfield: read enough real code to ratify conventions already in place — don't reinvent them

If a PRD or product-manager output exists, inherit settled requirements silently; don't re-decide product scope.

### Step 2: Frame the job

Confirm:

1. **Altitude** — whole system, feature/slice, or one epic/area
2. **Purpose & audience** — build substrate for implementers (default), discussion doc for alignment, or a narrower explainer
3. **Greenfield vs brownfield**
4. **Hard constraints** — cloud, language, compliance, timeline, team skills

If input is too thin for architecture (no problem, users, or scope), suggest finishing requirements with the `product-manager` skill first.

### Step 3: Discovery interview

Cover these areas, skipping anything already answered:

1. **Problem & success** — What must this architecture enable? What failure looks like?
2. **Boundaries** — System edges, external integrations, ownership seams
3. **Data & state** — What is shared? Who owns writes? Consistency needs?
4. **Scale & quality** — Throughput, latency, availability, security, tenancy — only real constraints
5. **Delivery** — Team shape, deploy model, environments, operability
6. **Risks** — What could force a rewrite? Riskiest technical assumption?

Elicit with open questions ("How are you thinking about X?"). Use crisp either/or only for genuine binary forks.

### Step 4: Play back and confirm

Before writing the architecture, summarize in a few bullets:

- Paradigm / approach
- Major boundaries
- Stack lean (or "ratify existing")
- Top 3 decisions and top deferred items

Ask the user to confirm or correct. Do not skip this step on the Coaching path.

### Step 5: Design and decide

For each load-bearing decision:

1. Name the divergence it prevents
2. List realistic alternatives (usually 2–3)
3. Recommend one with a short why
4. Lock it as `AD-n` only after user agreement (Coaching) or with `[ASSUMPTION]` / `[ADOPTED]` tags (Fast / already settled)

Prefer a named design paradigm (e.g. layered, hexagonal, event-driven, modular monolith) when it carries a coherent model for free.

For greenfield / beginner projects with an open stack, recommend a well-known current starter after verifying it on the web.

For brownfield, ratify existing patterns; surface conflicts instead of silently overriding them.

### Step 6: Write the architecture document

Use the template in [architecture-template.md](architecture-template.md). Write to a markdown file where the user prefers (default: `docs/architecture-<name>.md` in the project, or ask if no project is open).

- Every decision gets a stable ID (`AD-1`, `AD-2`, …) with **Binds**, **Prevents**, and **Rule**.
- Keep Structural Seed minimal — enough to start, owned by the code once it exists.
- Put non-decisions in Deferred with a revisit condition.
- Use Mermaid for structure when a diagram is clearer than prose; skip decorative diagrams.
- Sweep breadth the altitude owns: paradigm, boundaries, data ownership, key NFR envelopes, delivery/ops. Each dimension is decided, deferred, or an open question — never silently omitted.

### Step 7: Validate

Self-review before presenting:

- Does every `AD` prevent a real divergence?
- Could two implementers still choose incompatibly on something that should have been fixed?
- Are assumptions and open questions explicit?
- Are deferred items truly deferrable for the next phase of work?
- Do stack pins cite verified versions?
- Does brownfield guidance match the code that exists?

Present the doc, flag the weakest areas, and offer a revision round.

### Step 8: Handoff

After acceptance:

- Point to paths written
- Suggest next steps: epics/stories breakdown, spike on open questions, or UX/implementation collaboration
- Keep `AD` IDs stable across updates — amend rules in place; never renumber or reuse retired IDs

## Update & Validate Intents

**Update** an existing architecture: load the prior doc, append or amend decisions without renumbering IDs, re-run validation.

**Validate** only: critique an existing architecture against the checklist in Step 7; report findings by severity without rewriting unless asked.

## Collaboration

- Product scope and requirements → `product-manager`
- UX flows and interface behavior → `ux-designer`
- Do not edit application code unless the user asks to implement

## Additional Resources

- Architecture structure and section guidance: [architecture-template.md](architecture-template.md)
