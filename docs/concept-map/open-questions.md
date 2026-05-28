# Open Questions

A living list of questions I haven't answered yet. Each entry should be answerable, not philosophical.

## Convention

Format:

```
- [ ] (YYYY-MM-DD) <area>: <question>
      - Why this matters: …
      - What would resolve it: …
```

Move resolved questions into the relevant topic note as part of `interview-notes.md` or `senior-engineer-thinking.md` and check the box here.

---

## Backend systems

- [ ] (YYYY-MM-DD) Backend systems: When is dual-writing acceptable, and when is it always a smell? - Why this matters: I keep seeing it justified during migrations. - What would resolve it: An ADR with reversal triggers across at least three migration shapes.

## Distributed systems

- [ ] (YYYY-MM-DD) Distributed systems: What's the smallest set of patterns I can hold in my head that covers 80% of real-world async issues? - Why this matters: I want a working mental model, not a Bible. - What would resolve it: A 1-page note + 1 lab demonstrating each pattern.

## AI engineering

- [ ] (YYYY-MM-DD) AI engineering: What's the right default eval shape for a small RAG feature: rubric-based, dataset-based, or LLM-as-judge? - Why this matters: I don't want to overbuild evals, but no evals = theater. - What would resolve it: A benchmarked comparison across the three on a real corpus.

- [ ] (YYYY-MM-DD) AI engineering: How do I price an agent's per-task cost honestly when retries inflate token counts? - Why this matters: Cost attribution drives product decisions. - What would resolve it: A logged run on a benchmark task with cost broken down per retry.

## Data systems

- [ ] (YYYY-MM-DD) Data systems: When should I reach for OLAP vs continuing to push OLTP? - Why this matters: The wrong choice ossifies into expensive infra. - What would resolve it: A topic note + a small benchmark on a representative query mix.

## Observability

- [ ] (YYYY-MM-DD) Observability: What's the minimum useful set of signals for a tiny app? RED only? RED + traces? + cost? - Why this matters: Most "observability" advice assumes infinite scope. - What would resolve it: A 1-day lab on a tiny service that proves what's missed.

---

> Add questions liberally. They're cheap. Resolving them is the work.
