# System Design: <Problem>

## Problem statement

What are we designing? Who uses it? What does success look like?

## Functional requirements

Core capabilities the system must offer. Bullet list.

## Non-functional requirements

- Latency targets (p50, p95, p99)
- Throughput / QPS
- Availability target
- Consistency requirements
- Durability requirements
- Cost ceiling
- Security / compliance

## Back-of-envelope estimates

- Users
- Requests / sec
- Data volume / day
- Storage / year
- Bandwidth
- Cost order-of-magnitude

## High-level architecture

Components and their interactions. Add a sketch.

```
[ client ] -> [ edge / CDN ] -> [ api ] -> [ services ] -> [ datastores ]
                                            |-> [ queue ] -> [ workers ]
                                            |-> [ cache ]
```

## Data model

Key entities, relationships, ownership. Source of truth vs derived state.

## Key APIs

The 3–5 endpoints / interfaces that matter most.

## Critical paths

Hot paths in detail: read path, write path, search path, etc.

## Caching strategy

What is cached, where, with what invalidation.

## Consistency model

Strong, read-your-writes, eventual, causal? Justify the choice.

## Failure modes

Per-component: what fails, what's the blast radius, what's the mitigation.

## Scalability plan

What scales horizontally? What's the bottleneck? What's the next evolution?

## Observability

Logs, metrics, traces, dashboards, alerts. Per-component.

## Security

Trust boundaries. Auth, authz, tenant isolation. Abuse vectors.

## Cost model

Per-request and per-user cost. Where cost grows. Where to optimize first.

## Tradeoffs and alternatives

Decisions made, alternatives considered, reversal triggers. Link to ADRs.

## Open questions

Unresolved questions. Track durable ones in [`docs/concept-map/open-questions.md`](../../concept-map/open-questions.md).

## Practice notes

If this is a system design _practice_ (interview prep), include:

- Common follow-ups an interviewer would ask
- Things you forgot the first time
- Diagram(s) you should be able to draw from memory
