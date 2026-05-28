# Learning Backlog

Prioritized list of what to study. Re-rank monthly.

## Convention

- "Must deeply know" = professional staples; aim for Stage 9+ in the lifecycle.
- "Good to know" = recognize, can reach for; aim for Stage 4 in the lifecycle.
- "Ignore initially" = noise unless a specific problem demands it.

---

## Must deeply know

### Core engineering

- HTTP
- DNS
- TLS
- Networking basics (TCP, UDP, ports, NAT, sockets)
- Process / thread / event loop
- Concurrency (cooperative vs preemptive, async/await mental model)
- Memory (heap, stack, leaks, GC, allocators)
- Data structures (hash maps, trees, heaps, queues, sets, bloom filters)
- Algorithms (sorting, searching, BFS/DFS, dynamic programming, basics of complexity analysis)

### Backend systems

- API design (REST, gRPC, GraphQL — when each)
- Authentication (sessions, JWT, OAuth, OIDC)
- Authorization (RBAC, ABAC, ReBAC)
- Validation (parse, don't validate; schema at edges)
- Transactions
- Idempotency
- Background jobs
- Webhooks
- Rate limiting

### Data systems

- SQL (joins, window functions, CTEs)
- Indexes (B-tree, hash, GIN, GiST, vector)
- Query plans (EXPLAIN, statistics, planner gotchas)
- Migrations (online schema changes)
- Transactions (ACID, deadlocks)
- Isolation levels
- Replication (sync, async, logical, physical)
- Backups (point-in-time recovery, restore drills)

### Distributed systems

- Queues
- Retries (with backoff and jitter)
- Eventual consistency
- Consensus basics (Paxos / Raft mental model)
- Cache invalidation
- Distributed locks (and why to avoid them)
- Pub/sub
- Backpressure

### AI engineering

- LLM basics (tokenization, context window, sampling)
- Prompt design
- Structured outputs (JSON mode, schema-constrained generation)
- Embeddings
- RAG
- Vector search (ANN: HNSW, IVF)
- Reranking
- Evals (rubric-based, dataset-based, LLM-as-judge)
- Tool calling
- Model routing
- AI cost tracking
- Prompt injection

### Infrastructure

- Docker
- CI/CD
- Terraform basics
- Cloud deployment (compute, storage, identity)
- Secrets management
- Networking (VPCs, subnets, security groups)
- Load balancing
- CDN

### Observability

- Logs (structured, redaction)
- Metrics (RED, USE)
- Traces (OpenTelemetry)
- SLOs / SLIs / error budgets
- Dashboards
- Alerts (SLO-based, not arbitrary thresholds)
- Postmortems

### Security

- OWASP Top 10
- Auth security (token theft, replay, fixation)
- Tenant isolation
- Secrets handling
- SSRF
- XSS
- CSRF
- SQL injection
- Prompt injection

### Product engineering

- Onboarding flows
- Activation metrics
- Analytics & instrumentation
- Feedback loops (NPS, surveys, support → roadmap)
- Admin tools / internal tooling
- Pricing and packaging
- Support workflows

---

## Good to know

- Kafka
- Kubernetes
- Temporal (durable workflows)
- ClickHouse / OLAP engines
- CRDTs
- WebRTC
- Service mesh (Istio, Linkerd)
- Fine-tuning
- Local LLMs
- Multi-region systems
- Data pipelines (Airflow, dbt)
- Feature stores

---

## Ignore initially

- Microservices-first design
- Kubernetes-first deployment
- Premature Kafka
- Premature fine-tuning
- Autonomous agent swarms
- Blockchain / web3 (unless a specific problem demands it)
- Exotic databases without a clear access pattern
- "AI ops platforms" before having a real eval workflow
