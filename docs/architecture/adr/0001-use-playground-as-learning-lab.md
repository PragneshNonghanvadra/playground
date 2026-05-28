# ADR-0001: Use Playground as a learning lab, not a product

## Status

Accepted

## Date

2026-05-27

## Context

I am transitioning from a senior frontend-focused engineer into an AI-native full-stack / product engineer. The traditional path to that transition is to "build a project" — typically an AI-wrapper product. That path conflates _output_ with _judgment_.

What I actually need to develop is engineering judgment across many areas: backend, distributed systems, data, AI, infrastructure, observability, security, performance, product, and system design. A single product compresses these into one architecture, freezes its tradeoffs early, and makes me defend that architecture rather than continually critique it.

I also want to use AI coding agents seriously without becoming dependent on them. That requires deliberate practice in critiquing AI output, not just consuming it.

This repository, **Playground**, is the resulting structure: a long-term, modular learning lab.

## Decision

**Playground is structured as a learning operating system, not a product.**

Concretely:

1. The repo is **markdown-first**. Topic notes, ADRs, RFCs, interview notes, and tradeoffs are the primary deliverables.
2. Code lives in **small labs** that test one concept each, not in a single growing app.
3. `apps/`, `services/`, `packages/`, and `infra/` are placeholders. They become real only when a real product slice emerges from accumulated lab work.
4. AI coding agents are **collaborators, not architects**. Their outputs are logged and critiqued under `ai-agent/`.
5. The topic lifecycle (Stages 0–12) governs how a topic moves from curiosity to product integration. Most topics will not reach Stage 12, and that is fine.

## Alternatives considered

### Option 1: Build a single AI-native SaaS product

- **Pros**: Tangible artifact; portfolio-friendly; forces full-stack work.
- **Cons**: Early architectural choices become defended; learning compresses into the chosen domain; many concepts get skipped because the product doesn't need them; AI agents become productivity tools rather than subjects of critique.
- **Why not chosen**: It optimizes for shipping, not judgment.

### Option 2: Follow a structured curriculum / bootcamp

- **Pros**: External structure; benchmark progress against others.
- **Cons**: Curriculum-driven learning prioritizes coverage over depth; my goals are deeper-than-curriculum across selected areas; courses are tied to current frameworks rather than durable concepts.
- **Why not chosen**: I want depth over coverage and durable concepts over framework-of-the-month.

### Option 3: A pile of unstructured notes + side projects

- **Pros**: Low overhead.
- **Cons**: No connections between concepts; no lifecycle; experiments don't compound; AI critique disappears between sessions; quality drift.
- **Why not chosen**: No structural pressure to reach the high-leverage parts of learning (failure modes, tradeoffs, scaling).

## Consequences

### Positive

- Forces explicit articulation of mental models, tradeoffs, and failure modes.
- Builds compounding knowledge through cross-topic links.
- Creates a longitudinal record of AI-agent quality and my recalibration.
- Permits depth and breadth without committing to a single architecture.
- Templates and checklists keep quality high without ceremony.

### Negative

- Slower visible progress than building a product.
- Risk of "documentation theater" — writing about engineering instead of doing it.
- Requires discipline to actually _build_ labs, not just write notes.

### Neutral

- The structure must evolve. Folders may shift as my needs change. That's expected, not a sign the decision was wrong.

## Reversal triggers

I should revisit this decision if any of the following are true at a quarterly review:

- I haven't built a single lab in two consecutive months (notes-only drift).
- I'm shipping a product that needs a real production codebase, and the lab structure is genuinely in the way.
- I've reached Stage 12 in three or more topics and the modules are mature enough to demand a real app.
- A specific consulting or job opportunity requires a defendable single-product portfolio piece.

In any of those cases, I will write ADR-XXXX to either spawn a real product repo or restructure Playground as a hybrid.

## Related topics

- [`docs/operating-system/learning-principles.md`](../../operating-system/learning-principles.md)
- [`docs/operating-system/topic-lifecycle.md`](../../operating-system/topic-lifecycle.md)
- [`docs/operating-system/ai-agent-working-model.md`](../../operating-system/ai-agent-working-model.md)

## Related labs

_None yet — labs accumulate over time._

## Open questions

- What does "graduation" of a module from `labs/` to `packages/` look like in practice? (Capture in [`docs/concept-map/open-questions.md`](../../concept-map/open-questions.md).)
- When does a cluster of related modules deserve to become a real product slice? (Same.)
