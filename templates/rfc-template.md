# RFC: <Title>

## Summary

One paragraph. Someone reading only this should grasp the proposal.

## Problem

What is broken or missing today? Who feels it? How do we know?

## Goals

What does success look like? Measurable when possible.

## Non-goals

What is explicitly out of scope. Naming non-goals prevents scope creep and clarifies tradeoffs.

## Background

Relevant context: prior systems, related ADRs, topic notes, constraints, organizational realities.

## Proposed approach

The proposal in detail. Diagrams welcome.

## Alternatives considered

For each alternative: a summary, pros, cons, and why it isn't being proposed.

## Technical design

Concrete design: components, boundaries, ownership, interactions.

## Data model

Schemas, key entities, ownership. Note what is the source of truth and what is derived.

## APIs / contracts

Endpoints, message shapes, versioning. Idempotency, retries, error semantics.

## Failure modes

What can fail? What happens during partial failure? Retries, fallbacks, degradation modes.

## Security considerations

Trust boundaries. Auth and authorization. Tenant isolation. Secret handling. Abuse vectors. Prompt injection if AI is involved.

## Observability

Logs, metrics, traces, alerts. How will we debug this in production at 3am?

## Performance considerations

Latency targets, throughput targets, capacity, cost estimates. What's the bottleneck?

## Rollout plan

Migration steps. Feature flags. Phased rollout. Backfills. Compatibility window.

## Open questions

Questions still unresolved at the time of writing. Track durable ones in [`docs/concept-map/open-questions.md`](../../concept-map/open-questions.md).

## Decision

Final decision and link to the corresponding ADR once accepted.
