# Observability

## Why this topic matters

Observability is the difference between **debugging at 3am with confidence** and **guessing**. A system without observability is a system you cannot reason about under stress — and the time you most need to reason about it is exactly when you're least equipped to.

The 80/20 of observability is small but non-obvious:

- **Structured logs** with correlation IDs.
- **Metrics** that match either the **RED** (rate / errors / duration) or **USE** (utilization / saturation / errors) shape.
- **Traces** that span entry to exit across services.
- **SLOs** with error budgets, not arbitrary thresholds.
- **Dashboards** organized by question, not by metric.
- **Alerts** that fire only when humans should act.

For AI-native systems, observability extends to **LLM call traces**: prompts, models, tokens, latency, cost, tool calls, retries, and per-tenant attribution.

## Subtopics inside this area

- **Logs** — structured (JSON), redaction, correlation IDs, log levels, sampling.
- **Metrics** — counters, gauges, histograms; cardinality control; high-cardinality pitfalls.
- **Traces** — OpenTelemetry; spans, attributes, baggage; cross-service propagation.
- **Sampling** — head-based vs tail-based; cost vs fidelity.
- **SLOs / SLIs** — defining "what working means"; multi-window multi-burn-rate alerts.
- **Error budgets** — using SLO violations to slow shipping when reliability slips.
- **Dashboards** — overview vs drill-down; per-tenant; SLO views.
- **Alerts** — page vs ticket vs nothing; runbook coupling; alert fatigue.
- **Profiling** — continuous profiling; on-CPU vs off-CPU.
- **Tail latency** — p95, p99, p99.9; queueing; head-of-line blocking.
- **AI observability** — prompt logs, model versioning, token + cost attribution, eval drift, tool-call traces.
- **Distributed debugging** — trace IDs, log links, correlation across systems.
- **Postmortems** — blameless, timeline-based, action items with owners.

## How this connects to other topics

| Connects to             | How                                                                  |
| ----------------------- | -------------------------------------------------------------------- |
| **Backend systems**     | Every API has RED metrics; every endpoint has logs and traces.       |
| **Distributed systems** | Traces are the only sane way to debug across services.               |
| **Performance**         | p99 and tail latency are observability questions.                    |
| **Data systems**        | Slow query logs, plan changes, lock waits.                           |
| **AI engineering**      | Per-call token/cost/latency logs, eval regressions, drift detection. |
| **Security**            | Audit logs, PII redaction, intrusion detection.                      |
| **Product engineering** | Funnels, activation, feedback loops are observability-shaped.        |

## Suggested first experiments

1. **`labs/observability/logs-metrics-traces`** — Tiny service emitting structured logs, RED metrics, and OpenTelemetry traces. Generate load. Inject failure. Show how each signal helps debug.
2. **SLO + error budget** — Define an SLO ("99.5% of requests under 200ms"). Build a multi-window burn-rate alert. Inject latency. Watch the alert fire.
3. **Cardinality blow-up** — Add a `user_id` label to a metric; watch storage cost explode. Replace with a histogram + per-user trace sampling.
4. **AI call wrapper** — Wrap every LLM call with structured logging (model, prompt hash, tokens in/out, latency, cost, tag). Build a cost-per-feature dashboard.
5. **Postmortem dry run** — Run a chaos experiment. Write a postmortem from logs alone — _without_ asking the engineer who broke it. Notice what's missing.

## Interview relevance

Senior IC and on-call rotation interviews:

- "Walk me through how you'd debug a latency spike." (logs → metrics → traces → repro)
- "How would you set an SLO?" (multi-window burn rate, error budget, ownership)
- "What's the difference between observability and monitoring?" (knowable unknowns vs unknowable unknowns)
- "How do you avoid alert fatigue?" (SLO-based alerts; symptom alerts not cause alerts; runbooks)
- "How do you debug across services?" (trace IDs, structured logs, correlation)
- "How would you instrument an LLM-powered feature?" (prompt logs, token/cost, eval drift, per-tenant cost)

Strong answers center **the question being asked** and choose the right signal for it. Weak answers list tools.

## Topic notes in this folder

- `mental-model.md`, `why-it-matters.md`, `must-know.md`, `good-to-know.md`, `beginner-mistakes.md`, `senior-engineer-thinking.md`, `ai-native-perspective.md`, `production-concerns.md`, `failure-modes.md`, `scaling-considerations.md`, `tradeoffs.md`, `interview-notes.md`, `resources.md`, `experiments.md`, `connected-concepts.md`, `open-questions.md`.
