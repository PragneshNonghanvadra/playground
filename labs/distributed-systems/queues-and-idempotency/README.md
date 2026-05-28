# Lab: Queues and Idempotency

## Purpose

Internalize the most-common-real-world pattern in distributed systems: a producer-queue-worker pipeline that survives crashes, network drops, and duplicates. Build the muscle for "at-least-once delivery + idempotent consumer".

## Topic area

[`docs/topics/distributed-systems/`](../../../docs/topics/distributed-systems/)

## Learning goal

After this lab, I should be able to explain — with measurements — why "exactly once" is a marketing word and how at-least-once + idempotency keys + a dedupe store actually produce the _effect_ of exactly-once for users.

## Scope

- Tiny producer + queue + worker (in-memory or Redis or SQS).
- A dedupe store keyed by `idempotency_key`.
- Failure injection: worker crash mid-handle; duplicate messages; out-of-order delivery; slow worker (backpressure).
- Measurements: end-to-end latency, duplicate rate handled, DLQ rate.

## Non-goals

- Multi-region.
- Exotic queue features (priority, fairness scheduling).
- Comparing every queue tech.

## Architecture

```
[ producer ] -> [ queue ] -> [ worker ]
                                |- handler
                                |- dedupe store (Redis / Postgres)
                                |- DLQ (after N failures)
```

## Implementation plan

1. Pick a queue. Start with an in-memory channel or Redis Streams. Don't go straight to Kafka.
2. Producer sends messages with an `idempotency_key` (usually deterministic from business inputs).
3. Worker:
   - Pulls a message.
   - Looks up the key in the dedupe store. If present and successful, ACK and skip.
   - Otherwise, processes and stores `(key, result)` in the dedupe store.
   - On exception, NACK; queue retries with backoff.
   - After N retries, send to DLQ.
4. Add a small admin tool to replay messages from the DLQ.
5. Inject failures and measure.

## Commands

```bash
# placeholder; flesh out during implementation
# pnpm run producer
# pnpm run worker
# pnpm run inject-failures
```

## Expected observations

- With at-least-once delivery, a non-idempotent worker will sometimes process the same message twice.
- With idempotency keys + dedupe, duplicates become invisible to downstream effects (e.g., a charge is never doubled).
- Without backpressure, a slow worker will let the queue grow unbounded.
- Without DLQ, a poison-pill message will block progress forever.

## Failure experiment

- Kill the worker mid-handler; watch the message redeliver.
- Send the same message 100 times in parallel; verify only one downstream effect.
- Send a poison-pill message (always throws); verify it lands in DLQ after N retries.
- Drop the dedupe store while messages are in flight; observe the failure mode.

## Debugging guide

- Log every message with `id`, `idempotency_key`, `attempt`, `outcome`.
- Build a "trace" view: same `idempotency_key` across attempts.
- Track queue depth, oldest unacked age, DLQ rate.

## Benchmark plan

- Workload: 1000 msgs/sec sustained for 60s.
- Inject 10% failure rate.
- Measure: end-to-end latency p50/p95/p99, duplicate processing %, DLQ %.

## Production notes

What would change to take this real:

- Persistent queue (SQS / Redis Streams / Kafka / Rabbit).
- Persistent dedupe store with TTL longer than max retry window.
- Visibility timeouts tuned to handler duration.
- Per-tenant fairness if multi-tenant.
- DLQ alerting + replay tooling.
- Tracing across producer → queue → worker → downstream.

## Interview notes

- "Exactly once" is shorthand for "at-least-once delivery + idempotent consumer".
- Idempotency keys are the contract that makes retries safe.
- DLQ is the answer to "what about poison messages".
- Backpressure is what prevents queues from becoming the failure source.
- Visibility timeout is the most-misconfigured queue setting.

## AI-agent notes

What an AI agent might get wrong:

- Implement "exactly once" in the worker without acknowledging it requires queue + storage cooperation.
- Treat idempotency as deduping by message body hash — wrong when payloads have timestamps, retries with metadata changes, etc.
- Forget DLQ entirely.
- Use distributed locks instead of idempotency keys (different problem; locks have their own failure modes).
- Skip backpressure and recommend "just scale workers".

## Follow-up experiments

- Add ordered processing per key (e.g., per-user serialization) and measure throughput cost.
- Compare visibility-timeout vs explicit-lease semantics.
- Compare in-process backpressure (bounded channel) vs queue-side throttling.
- Add a saga: two queues, compensating actions on partial failure.

## Status

- [ ] Implementation pending
- [ ] First measurement taken
- [ ] Failure experiment run
- [ ] Lessons written back to `docs/topics/distributed-systems/`
