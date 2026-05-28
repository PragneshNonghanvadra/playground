# Concept Map

A living document that connects engineering concepts across this repo. When a new connection is noticed, add it here.

> Convention: each section is `## <Concept> connects to`, followed by a bullet list of related concepts with brief framing.

---

## Caching connects to

- **Frontend performance** — page load, hydration, optimistic UI
- **Backend scalability** — read amplification, hot keys, fanout
- **Database load** — query result caching, materialized views
- **Distributed consistency** — read-your-writes vs eventual
- **Cache invalidation** — TTL, write-through, write-behind, event-driven busts
- **CDN** — edge caching, cache keys, vary headers
- **Stale data UX** — when "good enough" beats "fresh"
- **Observability** — hit rate, miss rate, eviction rate, age-of-stale

## Queues connect to

- **Background jobs** — async work, scheduled tasks
- **Retries** — at-least-once delivery, exponential backoff
- **Idempotency** — duplicate handling, idempotency keys
- **Event-driven systems** — pub/sub, fanout, CDC
- **Distributed systems** — ordering, partitioning, backpressure
- **Webhooks** — outbound integration retries, replay tooling
- **Agent workflows** — durable steps, long-running orchestrations
- **Failure handling** — dead-letter queues, poison-pill messages

## Observability connects to

- **Debugging** — structured logs, correlation IDs, trace IDs
- **Reliability** — SLO/SLI, error budgets
- **Performance** — RED, USE, p50/p95/p99
- **AI traces** — prompt, model, tokens, latency, cost
- **Cost monitoring** — per-tenant, per-feature, per-model
- **Incident response** — runbooks, dashboards, alert routing
- **Production readiness** — what makes a system safe to deploy

## RAG connects to

- **Data ingestion** — chunking, embedding, indexing pipelines
- **Embeddings** — model choice, dimensionality, refresh strategy
- **Vector search** — ANN algorithms, filters, hybrid search
- **Permissions** — per-document ACL filtering at query time
- **Evals** — relevance, faithfulness, citation quality
- **Hallucination control** — grounding, abstention, refusal patterns
- **UX trust** — citations, confidence display, "why this answer"
- **Citations** — source linking, attribution UI

## AI agents connect to

- **Tool calling** — schema design, validation, error handling
- **Permissions** — what tools an agent can call, scoping
- **Workflow orchestration** — durable execution, retries, compensation
- **Sandboxing** — isolating side effects, resource limits
- **Evals** — task success rate, regression suites, cost per task
- **Observability** — step traces, prompts, decisions, retries
- **Security** — prompt injection, exfiltration, privilege escalation
- **Human approval** — when to require it, what UI shows the operator

## Authentication connects to

- **Authorization** — roles, attributes, policies
- **Session management** — cookies, JWTs, refresh tokens
- **Trust boundaries** — public vs internal, tenant vs cross-tenant
- **Security failure modes** — token leakage, replay, CSRF, fixation
- **Multi-tenancy** — tenant ID propagation, isolation
- **AI agent safety** — agent identity vs user identity, on-behalf-of patterns

## Indexes connect to

- **Query plans** — explain analyze, plan stability
- **Write amplification** — index maintenance cost
- **Read patterns** — predicate selectivity, covering indexes
- **Migrations** — concurrent index creation, rollout safety
- **Partitioning** — local vs global indexes
- **AI workloads** — vector indexes (HNSW, IVF), hybrid filters

## Idempotency connects to

- **Retries** — safe retry semantics
- **Queues** — at-least-once delivery
- **Webhooks** — idempotency keys for external callers
- **Payments** — duplicate-charge prevention
- **AI tool calling** — safe re-execution of tool calls

## Multi-tenancy connects to

- **Authorization** — tenant ID in every check
- **Data modeling** — tenant column vs schema-per-tenant vs db-per-tenant
- **Caching** — keys must include tenant ID
- **Search/embeddings** — filter by tenant before reranking
- **Rate limiting** — per-tenant quotas, fair-share scheduling
- **Observability** — per-tenant SLOs and cost attribution

---

> Add new sections as new connections become clear. Don't worry about taxonomy — clarity over neatness.
