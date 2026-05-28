# Coding Agent Rules

Hard rules for any AI coding agent operating in this repo. When in doubt, refuse and ask.

## Repo posture

- This is a **learning lab**, not a production app.
- Markdown-first. Code is in service of learning, not the other way around.
- Boring tech by default. Novel tech only when the lab is _about_ novelty.
- Prefer monolithic structure unless a lab specifically explores microservices.

## Process rules

1. **Before non-trivial changes, read** [`instructions.md`](instructions.md) and the affected topic folder.
2. **Restate the goal** before acting. If the goal is ambiguous, ask.
3. **Plan first**. Write the plan in the chat. Then implement.
4. **Small commits**. One concept per change.
5. **Update docs alongside code**. New lab → topic note updated. New module → ADR drafted. New decision → reversal triggers named.
6. **Run the relevant** [review checklist](../docs/operating-system/review-checklists.md) **before stopping.**

## Architecture rules

- Every architectural decision must name **at least three alternatives** and **reversal triggers**.
- Every distributed pattern must name **failure modes**, **idempotency posture**, and **observability**.
- Every cache must have a documented **invalidation strategy**.
- Every queue must define **at-least-once vs exactly-once posture**, **retry behavior**, and **dead-letter handling**.
- Every database choice must justify **access patterns** and **consistency requirements**.

## AI/LLM rules

- No LLM call without **structured logging** (model, tokens, latency, cost).
- No LLM call without an **eval or rubric plan** (even if "rubric: human spot check; will upgrade").
- No agent loop without **explicit step limits, timeouts, and a kill switch**.
- No tool calling without **input validation and an allowlist of tools**.
- No prompt without **prompt-injection consideration** when user input is concatenated.

## Code style

- TypeScript strict mode where TS is used.
- Validate inputs at trust boundaries (e.g., Zod, Valibot, runtime checks at API edges).
- Errors are typed or wrapped — no swallowed exceptions.
- Logs are structured. No `console.log` in non-throwaway code (except in `labs/` where the trade-off is explicit).
- Comments explain _why_, not _what_. Avoid narrating the code.

## Forbidden by default (require explicit user approval)

- Adding Kubernetes, Helm, service mesh, Istio, Kafka, Cassandra, or similar without a corresponding lab/topic about _that exact tool_.
- Splitting code into microservices.
- Adding multi-region infrastructure.
- Introducing a new language or runtime.
- Generating large amounts of mock data inline (use `evals/datasets/` if datasets are needed).
- Auto-generating ADRs without a clear decision having been discussed.

## Output rules

- Keep responses focused. Don't lecture the user about things they already know.
- Surface uncertainty explicitly: "I'm not sure whether X — verify with primary source Y."
- When proposing an architecture, default to the smallest version that proves the idea, then optionally describe how to evolve it.

## When asked something outside scope

- Say so.
- Suggest the smallest version of the work that fits the lab's purpose.
- Or propose deferring with a note in [`docs/concept-map/learning-backlog.md`](../docs/concept-map/learning-backlog.md).
