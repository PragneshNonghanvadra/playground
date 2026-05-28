# Distributed Systems

## Why this topic matters

The moment a system has **more than one process talking to another over a network**, it is a distributed system — whether you wanted one or not. That includes a web app talking to a database, a service calling a third-party API, a frontend hitting a backend across the internet, and an LLM client invoking a tool.

The fundamental shift: **partial failure becomes the default, not the exception**. Calls can succeed, fail, or — most dangerously — _time out without you knowing what actually happened on the other side_.

Most senior-level mistakes in modern apps are distributed-systems mistakes wearing the costume of a "small" feature: dual writes that desync, retries that double-charge, caches that lie, queues that lose work, agents that loop. Learning the small set of patterns that handle partial failure pays off across every backend, every AI workflow, and every system design interview.

## Subtopics inside this area

- **Network reality** — fallacies of distributed computing; "the network is reliable" is always wrong.
- **Time** — clock skew, monotonic vs wall clocks, logical clocks (Lamport, vector).
- **Consistency models** — strong, sequential, causal, eventual; read-your-writes.
- **Consensus** — what Paxos / Raft solve; when you need them; when you don't.
- **Replication** — sync vs async, leader/follower, multi-leader, quorum.
- **Partitioning / sharding** — hash, range, directory; rebalancing; hot keys.
- **Queues** — at-least-once, at-most-once, exactly-once-effective; ordering guarantees.
- **Idempotency** — keys, dedupe windows; the lever that makes at-least-once safe.
- **Retries** — backoff, jitter, retry storms, circuit breakers.
- **Backpressure** — when consumers can't keep up; bounded queues; load shedding.
- **Distributed locks** — and the many reasons to avoid them.
- **Pub/sub** — fanout, ordering, replay, schema evolution.
- **Outbox pattern** — durable outbound messaging without dual-write hell.
- **Saga / compensation** — long-running workflows without distributed transactions.
- **Failure detection** — heartbeats, leases, gossip, phi-accrual.
- **CAP / PACELC** — what they actually mean (and don't).

## How this connects to other topics

| Connects to               | How                                                                                                |
| ------------------------- | -------------------------------------------------------------------------------------------------- |
| **Backend systems**       | Idempotency, retries, queues, webhooks all live here.                                              |
| **Data systems**          | Replication, isolation, transactions are distributed-systems problems.                             |
| **AI agents & workflows** | Agent orchestration is a durable-workflow problem. Retries, compensation, observability all apply. |
| **Observability**         | You cannot debug distributed systems without traces, correlation IDs, and structured logs.         |
| **Security**              | Trust boundaries multiply with services; auth propagation, agent identity.                         |
| **Performance**           | Tail latency dominates; fanout amplifies p99 into "p99 of N attempts".                             |
| **System design**         | Every interview question is implicitly a distributed-systems question.                             |

## Suggested first experiments

1. **`labs/distributed-systems/queues-and-idempotency`** — Producer + queue + worker. Inject failures (worker crash, network drop, slow worker). Measure duplicate handling. Implement an idempotency key store. Build a DLQ.
2. **Outbox pattern** — DB write + queue publish, atomically. Inject crashes between the two. Verify no message is lost or doubled.
3. **Saga over two services** — Order-and-payment style. Compensating transactions on failure.
4. **Tail latency under fanout** — One request that fans out to N backends. Show how p99 of fanout depends on the number of backends.
5. **Distributed lock vs lease** — Implement both for the same critical section. Inject network partitions. Notice the failure modes of locks.

Each lab should include a failure experiment and a measurement.

## Interview relevance

Senior backend, infra, and system-design interviews assume working knowledge of these patterns. Common questions:

- "Design a payments system." (idempotency, exactly-once-effective, sagas)
- "How do you ensure a webhook is delivered exactly once?" (you don't — you make the receiver idempotent)
- "Explain CAP." (and avoid the rookie answer; explain PACELC for bonus points)
- "How does Raft work?" (mental model, not algorithm proof)
- "Walk through a deploy of a stateful service." (rolling, drain, leader election)
- "How would you build a job scheduler?" (durability, leases, fairness, retries)
- "How do you debug intermittent failures across services?" (structured logs, trace IDs, correlation across spans)

Strong answers center **partial failure**, **idempotency**, and **observability**. Weak answers list tools without explaining what they're solving.

## Topic notes in this folder

- `mental-model.md`, `why-it-matters.md`, `must-know.md`, `good-to-know.md`, `beginner-mistakes.md`, `senior-engineer-thinking.md`, `ai-native-perspective.md`, `production-concerns.md`, `failure-modes.md`, `scaling-considerations.md`, `tradeoffs.md`, `interview-notes.md`, `resources.md`, `experiments.md`, `connected-concepts.md`, `open-questions.md`.
