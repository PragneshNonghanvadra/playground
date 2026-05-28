# Production Readiness: <Service / Module / Lab>

## Owner

Who owns this in production? Primary, backup, escalation.

## Purpose

What does this do, and what user/system problem does it solve?

## Dependencies

Upstream services, databases, queues, third-party APIs, models.

## Trust boundaries

Where does untrusted input enter? Where does data cross tenant or user boundaries?

## Auth & authz

How are callers authenticated? What permissions are checked?

## Validation

Where is input validated? What schemas? What happens on invalid input?

## Idempotency

Which operations are idempotent? How is idempotency enforced (keys, dedupe windows)?

## Failure handling

- Retries (where, how many, backoff strategy)
- Timeouts (per call, end-to-end)
- Circuit breakers
- Fallbacks / degradation

## Observability

- Structured logs (what fields, what redaction)
- Metrics (RED: rate, errors, duration; USE: utilization, saturation, errors)
- Traces (entry points, spans, correlation IDs)
- Dashboards (links)
- Alerts (thresholds, runbooks)

## SLOs / SLIs

- Availability target
- Latency target (p50, p95, p99)
- Error budget policy

## Capacity & cost

Expected load. Headroom. Cost per request. Cost per user. What grows linearly vs sublinearly?

## Data

Source of truth. Backups. Retention. Migrations. Sensitive fields. Encryption (at rest, in transit).

## Security

OWASP review. Secret handling. SSRF, XSS, CSRF, SQLi as applicable. For AI: prompt injection, output validation, tool sandboxing.

## Multi-tenancy

How is tenant isolation enforced? Can tenant A see tenant B's data via a bug, cache, log, search index, or AI memory?

## Deploy & rollback

Deploy strategy (blue/green, canary, rolling). Rollback plan. Migration reversibility. Feature flags.

## Runbooks

Common incidents and the steps to resolve them.

## Known gaps / future work

Honest list of what's not yet ready. Track in [`docs/concept-map/open-questions.md`](../../concept-map/open-questions.md) if durable.
