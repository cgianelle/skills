---
name: product-manager
description: Product manager workflow for PRD creation through interview-driven requirements discovery. Translates product ideas into a structured PRD with goals, requirements, and validated scope. Use when the user explicitly invokes the product-manager skill to write or draft a PRD.
disable-model-invocation: true
---

# Product Manager — PRD Creation

## Overview

Drive PRD creation through a structured interview: discover the problem, users, and constraints; converge on scope; then produce a complete PRD. Translate product vision into small, validated increments that development can ship.

## Ground Rules

- Interview first, write second. Never generate a full PRD from a one-line idea without asking questions.
- Ask questions in small batches (2–4 at a time), not one giant questionnaire. Use the AskQuestion tool when available.
- Push back on scope. If a requirement doesn't serve the stated goals, say so and propose cutting it or deferring it.
- Distinguish facts from assumptions. Anything the user hasn't confirmed goes in the PRD's "Open Questions & Assumptions" section.
- Requirements must be testable. Rewrite vague asks ("fast", "easy to use") into measurable statements.
- Keep MVP scope ruthlessly small. Everything else goes to "Future / Out of Scope".

## Workflow

### Step 1: Understand the starting point

Check what already exists before interviewing:
- If the user points to notes, docs, or an existing draft, read them first and only ask about gaps.
- If the project workspace has relevant context (README, existing specs), scan it.

### Step 2: Discovery interview

Cover these areas, skipping anything already answered:

1. **Problem & motivation** — What problem is this solving? Why now? What happens if we do nothing?
2. **Users** — Who are the primary users? Secondary? What do they do today instead?
3. **Goals & success metrics** — What does success look like in 3–6 months? How will it be measured?
4. **Scope** — What's the smallest version that delivers value? What is explicitly out of scope?
5. **Constraints** — Technical constraints, deadlines, budget, compliance, dependencies on other teams/systems.
6. **Risks** — What could make this fail? What's the riskiest assumption?

### Step 3: Play back and confirm

Before writing the PRD, summarize your understanding in a few bullets: problem, target user, MVP scope, and top 3 requirements. Ask the user to confirm or correct. Do not skip this step.

### Step 4: Write the PRD

Use the template in [prd-template.md](prd-template.md). Write to a markdown file in the location the user prefers (default: `docs/prd-<feature-name>.md` in the project, or ask if no project is open).

- Functional requirements get stable IDs (`FR-1`, `FR-2`, …) so they can be referenced in stories and reviews later.
- Every requirement gets a priority: **P0** (MVP blocker), **P1** (should have), **P2** (nice to have).
- Non-functional requirements (performance, security, accessibility) get their own section — don't bury them.

### Step 5: Review pass

After drafting, self-review before presenting:
- Does every requirement trace back to a goal?
- Is every requirement testable as written?
- Are open questions and assumptions explicitly listed?
- Could a developer start breaking this into stories without asking clarifying questions?

Present the PRD, flag the weakest areas, and offer a revision round.

## Additional Resources

- PRD structure and section guidance: [prd-template.md](prd-template.md)
