# Lab: HTTP API Basics

## Purpose

Build a tiny HTTP API by hand to internalize the request/response cycle, idempotency, status semantics, and rate limiting — without relying on framework magic.

## Topic area

[`docs/topics/backend-systems/`](../../../docs/topics/backend-systems/)

## Learning goal

After this lab, I should be able to explain (and demonstrate) what each part of an HTTP request and response is doing, why idempotency keys exist, what makes a `POST` vs `PUT` choice meaningful, and how a basic rate limiter behaves under load.

## Scope

- A single-file HTTP server (Node/Bun/Go — pick one) handling 3-5 endpoints by hand.
- Manual JSON parsing and response shaping.
- Idempotency key on at least one mutating endpoint.
- A simple in-memory rate limiter (token bucket).
- Structured logs.

## Non-goals

- Frameworks (Express, Fastify, Nest, etc.). The whole point is to feel the primitives.
- Auth. Add later in a separate lab.
- Persistence. In-memory is fine.
- TLS. Run plain HTTP locally.

## Architecture

```
[ curl / k6 ] -> [ tiny http server ]
                    |- routes
                    |- idempotency key store (in-memory map)
                    |- token-bucket rate limiter
                    |- structured logger
```

## Implementation plan

1. Stand up a server with a single `GET /healthz`.
2. Add `POST /items` with input validation. Return `201` and the resource.
3. Add idempotency: client sends `Idempotency-Key`; server stores `(key -> response)` for a TTL.
4. Add `GET /items/:id` and `PUT /items/:id`.
5. Add a token-bucket rate limiter keyed by IP.
6. Add structured logs (JSON line per request: method, path, status, duration_ms, ip, idempotency_key).
7. Add a tiny load test (`k6` or `oha` or `wrk`).

## Commands

```bash
# scaffold (placeholder; flesh out when implementing)
# pnpm init / go mod init / etc.

# run
# pnpm start

# load test
# k6 run scripts/load.js
```

## Expected observations

- `Idempotency-Key` returns the same response on duplicate POST.
- Rate limiter returns `429` once budget is exhausted; recovers after refill window.
- Latency is dominated by JSON parse/format and console logging at high QPS.

## Failure experiment

- Send malformed JSON → server should return `400`, never crash.
- Send 1k concurrent POSTs with the **same** `Idempotency-Key` and slightly different bodies — what happens? (Hint: define the policy explicitly. Strict-match? Last-writer-wins? Reject?)
- Send a request, kill the process before responding, restart, replay the request — does the replay succeed cleanly?

## Debugging guide

- Tail the structured log; grep by `idempotency_key`.
- Track per-route p50/p95/p99 from log durations.
- Add a `?debug=1` query param that includes timing breakdowns in the response.

## Benchmark plan

- Workload: 100 concurrent users, 30s, mix of `GET /items/:id` and `POST /items`.
- Metrics: p50, p95, p99 latency; throughput (RPS); error rate; rate-limited rate.
- Target: define a baseline first; don't optimize without it.

## Production notes

What would change to take this real:

- Auth (sessions or JWTs) at the trust boundary.
- Persistent idempotency-key store (Redis or Postgres) with TTL.
- Distributed rate limiting (Redis or token leasing).
- Structured logs to a sink (stdout → vector → log backend).
- Metrics (prometheus), traces (OTel), dashboards.
- Multi-tenant: keys must include tenant ID.
- Schema versioning on responses.

## Interview notes

- Why `POST` vs `PUT`: idempotency semantics, not "create vs update".
- Idempotency key: makes at-least-once delivery safe end-to-end.
- Rate limiting: token bucket vs leaky bucket vs fixed window — which and why.
- The most common bug in DIY APIs: not handling truncated/large bodies; double-handling errors; not setting `Content-Length`.

## AI-agent notes

What an AI agent might get wrong:

- Reach for a heavy framework when "by hand" is the point.
- Skip idempotency entirely, or implement it as a hash of the body (which is _not_ idempotency — it's de-duplication, and breaks when keys are explicit).
- Implement rate limiting with naive counters (race conditions; not refill-aware).
- Log unstructured strings instead of JSON.

## Follow-up experiments

- Add server-sent events / streaming for a long-running endpoint.
- Compare HTTP/1.1 vs HTTP/2 keep-alive characteristics.
- Add a slowloris-style failure experiment.
- Add gRPC variant; compare wire size and latency.

## Status

- [ ] Implementation pending
- [ ] First measurement taken
- [ ] Failure experiment run
- [ ] Lessons written back to `docs/topics/backend-systems/`
