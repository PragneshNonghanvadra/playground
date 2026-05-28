# Lab: Logs, Metrics, Traces

## Purpose

Build the smallest meaningful observability stack on a single service to internalize what each signal does — and what it _cannot_ do.

## Topic area

[`docs/topics/observability/`](../../../docs/topics/observability/)

## Learning goal

After this lab, I should be able to debug a synthetic incident using the three signals correctly: logs for the _what_, metrics for the _how often / how slow_, traces for the _where in the call graph_.

## Scope

- A tiny HTTP service with 2-3 endpoints.
- Structured JSON logs with a correlation ID.
- Prometheus-format metrics (RED: rate, errors, duration).
- OpenTelemetry traces exported locally (Jaeger / Tempo / OTel Collector).
- A synthetic load generator that injects failures and latency.

## Non-goals

- Distributed tracing across services. Single service first.
- Multi-region.
- Production-grade alerting. A single SLO + burn-rate alert is enough.

## Architecture

```
[ load gen ] -> [ service ] -> [ db / dependency ]
                  |
                  |-> stdout JSON logs -> log viewer
                  |-> /metrics (Prom) -> Prometheus -> Grafana
                  |-> OTel SDK -> OTel Collector -> Jaeger
```

## Implementation plan

1. Service: 2-3 endpoints. One depends on a fake "DB" (a function with controllable latency and error rate).
2. Logs: structured JSON, with `request_id`, `method`, `path`, `status`, `duration_ms`, `tenant_id` if present.
3. Metrics: histogram on request duration; counter on requests + errors. Labels kept low-cardinality (route, status_class).
4. Traces: a span per request, child span per dependency call, attributes for tenant/user/path.
5. Compose-up Prometheus, Grafana, Jaeger, OTel Collector.
6. Load generator that varies QPS, error rate, and dependency latency.

## Commands

```bash
# placeholder; flesh out during implementation
docker compose up -d
# pnpm start
# pnpm run loadgen --rps 100 --error-rate 0.05
# open http://localhost:3000  # grafana
# open http://localhost:16686 # jaeger
```

## Expected observations

- A latency spike shows in metrics first; logs explain why on individual requests; traces show where in the call graph time was spent.
- Cardinality matters: adding `user_id` as a metric label blows up Prometheus storage.
- A naive p95 hides head-of-line blocking; look at p99 and p99.9.
- An SLO-based alert is far less noisy than a static-threshold alert.

## Failure experiment

- Inject 500ms latency into the dependency. Watch p99 spike before any errors.
- Inject 10% errors at the dependency. Watch RED metrics and traces correlate.
- Add a high-cardinality label. Observe the resource hit on Prometheus.
- Disable trace sampling at high QPS — see what falls over (collector, network, disk).
- Generate a synthetic incident and write a postmortem from observability data alone.

## Debugging guide

- Start at metrics: which endpoint, what's the change, when did it start?
- Drop into logs filtered by route + status class to read raw failures.
- Pick one bad request_id; pull its trace; identify the slow span.
- Decide what to fix; verify with the same signals.

## Benchmark plan

- Workload: 100 RPS for 60s, mix of fast / slow / error endpoints.
- Capture: p50/p95/p99 from metrics; sampled traces for slow requests; correlation across signals.
- Confirm an SLO-based alert (e.g., "error rate > X% for Y minutes") fires correctly.

## Production notes

- Log redaction for PII at the application layer, before transport.
- Tail-based sampling for traces to keep slow/error traces while dropping the rest.
- Per-tenant SLOs and dashboards.
- AI extension: wrap LLM calls with a span (model, prompt-hash, tokens, latency, cost) and log them.
- Cost controls: metrics cardinality, trace sampling, log volume.

## Interview notes

- Logs vs metrics vs traces: each answers a different question.
- RED for request-driven services; USE for resources.
- High cardinality is the silent killer of metrics systems.
- SLO-based alerting beats static thresholds.
- Traces are the only sane way to debug across services.

## AI-agent notes

What an AI agent might get wrong:

- Suggest "just add a log" without structured fields.
- Add `console.log` everywhere in production code.
- Use unbounded labels on metrics.
- Skip traces "for simplicity" and lose the only tool that explains cross-component latency.
- Recommend an alert rule like "p99 > 500ms for 1 minute" — guaranteed page fatigue.

## Follow-up experiments

- Multi-service: extend the trace across two services.
- Add an SLO + burn-rate alert with multi-window logic.
- Add LLM-call instrumentation and a per-tenant cost dashboard.
- Try tail-based sampling and measure trace fidelity vs cost.

## Status

- [ ] Implementation pending
- [ ] First measurement taken
- [ ] Failure experiment run
- [ ] Lessons written back to `docs/topics/observability/`
