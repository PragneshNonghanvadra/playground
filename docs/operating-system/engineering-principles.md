# Engineering Principles

These are the rules I use to make engineering decisions in this repo (and beyond).

## 1. Boring tech by default

Reach for novelty only when the lab is _about_ novelty. Boring tech has known failure modes, mature ops, and a deep talent pool.

## 2. Make the simplest version that proves the idea

The smallest working version is the most informative. Add complexity only when you can name the problem it solves.

## 3. Name the source of truth

Every system has one. If I can't name it, the design is wrong.

## 4. Make state explicit

What's durable? What's cached? What's derived? What's in-flight? Hidden state is where bugs live.

## 5. Idempotency is a feature, not a luxury

Anything across a network boundary should be idempotent or have a clear plan for what duplicates do.

## 6. Design for partial failure

In any non-trivial system, _something_ is failing right now. Design for it: timeouts, retries (with backoff and idempotency), fallbacks, circuit breakers.

## 7. Trust boundaries are first-class

Where does untrusted input enter? Where does data cross tenant or user boundaries? Mark these explicitly.

## 8. Observability is a design choice

Logs, metrics, traces, and dashboards are not "ops concerns" added later. They are designed alongside the system.

## 9. Reversibility beats correctness

Most decisions can be wrong if they are easy to reverse. Optimize for reversibility; reserve correctness fights for irreversible decisions.

## 10. Optimize for reading

Code is read more than written. Notes are read more than written. Architecture diagrams are read more than drawn. Optimize all three for the reader.

## 11. Cost is a design constraint

Latency, correctness, and cost are co-equal. AI workloads especially: log token usage, eval cost, infra cost — early.

## 12. Microservices are a last resort

Not a starting point. Most systems should start as a well-organized monolith.

## 13. Caching is a contract

Caches are not a free speedup; they are a stale-data tradeoff. Define what may be stale, for how long, and how invalidation works.

## 14. Migrations are designed, not improvised

Plan the read path, the write path, the dual-write window, the cutover, and the rollback. Before you start.

## 15. AI without evals is theater

If LLM outputs affect users, there must be an eval plan, a logging plan, and a hallucination-handling plan. Otherwise, you're guessing.

## 16. Naming matters

If a concept is hard to name, it's probably two concepts. If a function is hard to name, it's probably doing two things.

## 17. The team is part of the system

A system maintainable by 3 people may collapse with 30. Account for the people in your architecture choices.

## 18. Beware "configurability" as a substitute for thinking

Every config knob is a future bug, a future doc, and a future support ticket.
