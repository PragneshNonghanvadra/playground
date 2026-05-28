# Lab: <Lab Name>

## Purpose

What concept this lab is designed to teach. One or two sentences.

## Topic area

Which topic this belongs to (link to `docs/topics/<topic>/README.md`).

## Learning goal

What I should understand after completing this lab. Phrased as: "After this lab, I should be able to explain / measure / build **\_**."

## Scope

What this lab includes. Be aggressive about keeping this small.

## Non-goals

What this lab intentionally ignores. Naming non-goals prevents scope creep.

## Architecture

Add a small diagram (ASCII is fine) or written explanation.

```
[ client ] -> [ component under test ] -> [ dependency ]
```

## Implementation plan

Small, ordered steps. Each step should be runnable on its own.

1. Step 1
2. Step 2
3. Step 3

## Commands

How to run it.

```bash
# install
pnpm install

# run
pnpm --filter <lab> dev

# test
pnpm --filter <lab> test
```

## Expected observations

What behavior I should notice. Be specific — "request takes ~50ms p50, ~200ms p99 under 100 concurrent connections" beats "should be fast".

## Failure experiment

How to intentionally break it. Every non-trivial lab needs at least one. Examples: kill the dependency, fill the disk, flood with traffic, inject latency, send malformed input, drop network packets.

## Debugging guide

How to inspect what happened. Logs to grep, metrics to chart, traces to look at, queries to run.

## Benchmark plan

How to measure performance, latency, memory, cost, or correctness. State the workload, the metric, and the target.

## Production notes

What would need to change in a real system. Auth, multi-tenancy, retries, observability, durability, secrets, rollback strategy, etc.

## Interview notes

How to explain this lab in an interview. 30-second pitch + 2-3 things you learned.

## AI-agent notes

What AI agents may get wrong when implementing this. Common hallucinations, missed tradeoffs, premature abstractions.

## Follow-up experiments

What to try next. Variants, harder versions, comparisons against alternatives.
