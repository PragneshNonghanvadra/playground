# Terms

Alphabetical. Keep entries short and unambiguous. Link to topic notes for depth.

## A

- **ACID** — Atomicity, Consistency, Isolation, Durability. Properties of database transactions.
- **ANN (Approximate Nearest Neighbor)** — Algorithms (HNSW, IVF) that trade recall for speed in vector search.

## B

- **Backpressure** — When a downstream component can't keep up; signals upstream to slow down (or fails).
- **B-tree** — Self-balancing tree used as the default index structure in most relational databases.

## C

- **CAP** — Consistency, Availability, Partition tolerance. Pick two — but in practice, partition tolerance is assumed; the real trade is C vs A.
- **CDC (Change Data Capture)** — Streaming database changes to downstream consumers.
- **Circuit breaker** — Pattern that stops calls to a failing dependency to prevent cascading failure.

## D

- **DLQ (Dead Letter Queue)** — Where messages go after exhausting retries.

## E

- **Eventual consistency** — Reads may temporarily return stale data; given time, all replicas converge.

## I

- **Idempotency** — Performing the same operation N times has the same effect as performing it once.

## R

- **RAG (Retrieval-Augmented Generation)** — Retrieving documents to ground LLM generation.
- **RED metrics** — Rate, Errors, Duration. The minimum useful instrumentation for request-driven services.

## S

- **Saga** — A long-running workflow broken into steps with compensating actions for rollback.
- **SLO (Service Level Objective)** — Target reliability level (e.g., 99.5% of requests under 200ms).

## U

- **USE metrics** — Utilization, Saturation, Errors. The minimum useful instrumentation for resources.

> Stub. Add entries as terms recur in notes.
