# Backend Systems

## Why this topic matters

Backend systems are where the **truth** of an application lives. The frontend is a view; the backend is the system of record. If the backend is wrong, no UI polish saves you. Multi-user products live or die on three backend questions:

1. **Who is allowed to do what?** (auth + authz)
2. **What is the source of truth?** (data model + transactions)
3. **What happens when something goes wrong?** (idempotency, retries, observability)

Most "backend bugs" are actually skipped answers to one of those three.

For an AI-native product engineer, backend systems also become the **trust boundary** between humans and agents. Tool calling, agent state, durable workflows, and per-user permissions all sit in backend territory.

## Subtopics inside this area

- **API design** — REST, gRPC, GraphQL; resource modeling; versioning; pagination; idempotency; error semantics.
- **Authentication** — sessions, JWTs, OAuth/OIDC, refresh tokens, passkeys.
- **Authorization** — RBAC, ABAC, ReBAC; policy engines; row-level vs column-level.
- **Validation** — schemas at edges (Zod, Valibot, Pydantic), parse-don't-validate.
- **Transactions** — ACID, isolation levels, locking; what's safe inside a transaction vs outside.
- **Idempotency** — keys, dedupe windows, exactly-once-effective semantics.
- **Background jobs** — queues, schedulers, durable workflows.
- **Webhooks** — signing, retries, idempotency keys, replay tooling.
- **Rate limiting** — token bucket, leaky bucket, fixed window; per-user vs per-tenant vs per-IP.
- **Multi-tenancy** — column-level vs schema-level vs database-level isolation.
- **Pagination** — offset vs keyset; cursor design; consistency under writes.
- **Caching** — read-through, write-through, write-behind; invalidation strategies; stampedes.
- **API contracts** — OpenAPI, Protobuf; contract testing; client/server drift.

## How this connects to other topics

| Connects to               | How                                                                                                       |
| ------------------------- | --------------------------------------------------------------------------------------------------------- |
| **Data systems**          | Every backend non-trivially depends on database internals: indexes, transactions, isolation.              |
| **Distributed systems**   | Retries, idempotency, queues, eventual consistency — backend lives inside distributed systems by default. |
| **Security**              | Trust boundaries, tenant isolation, prompt injection in AI features.                                      |
| **Observability**         | RED/USE metrics, structured logs, traces, SLOs are backend hygiene.                                       |
| **Performance**           | Latency budgets, p99, fanout, caching strategy.                                                           |
| **AI engineering**        | Tool calling, agent permissions, durable workflows, per-user data access for RAG.                         |
| **Frontend architecture** | API contract shapes UI; pagination & caching strategies leak into UX.                                     |

## Suggested first experiments

1. **`labs/backend-systems/http-api-basics`** — Build a minimal HTTP API by hand. No framework magic. Learn requests, responses, status codes, content negotiation, error semantics, idempotency keys, and rate limiting from first principles.
2. **Idempotent endpoint with a dedupe store** — Implement an idempotency key on a `POST /payments` endpoint, with an external Redis dedupe store. Measure duplicate-request behavior under concurrency.
3. **JWT vs session auth** — Same login flow with two backends; compare revocation, blast radius of token theft, and operational burden.
4. **Background job + retries + DLQ** — Single-worker queue with exponential backoff, jitter, and a dead-letter queue. Inject failures.
5. **Webhook receiver** — Signature verification, idempotency, retry-friendly response semantics.

Each lab should include a measurement and a failure experiment.

## Interview relevance

This area is dense with interview questions across senior IC roles, especially:

- "Design a URL shortener." (rate limiting, idempotency, pagination)
- "Design a payments retry system." (idempotency, exactly-once-effective)
- "Design a webhook receiver." (signing, idempotency, retries)
- "Walk me through how a request gets authenticated." (TLS, JWT vs session, refresh, revocation)
- "How would you implement multi-tenancy?" (data modeling, tenant ID propagation, blast radius)
- "How do you make a job queue durable?" (at-least-once, idempotency keys, DLQ, poison pills)

Strong answers always name **failure modes**, **idempotency posture**, and **rollback/retry semantics**. Practice 30-second, 2-minute, and deep-dive versions of each.

## Topic notes in this folder

- `mental-model.md` — the simplest durable picture
- `why-it-matters.md` — first-principles rationale
- `must-know.md` — the irreducible vocabulary
- `good-to-know.md` — adjacent recognitions
- `beginner-mistakes.md` — common shallow misunderstandings
- `senior-engineer-thinking.md` — how seniors reason here
- `ai-native-perspective.md` — what changes when AI agents enter the picture
- `production-concerns.md` — what breaks in production
- `failure-modes.md` — concrete failures
- `scaling-considerations.md` — what changes at 10x/100x/1000x
- `tradeoffs.md` — competing approaches
- `interview-notes.md` — interview-friendly framings
- `resources.md` — books, papers, blogs, repos
- `experiments.md` — labs to build (and what they'll teach)
- `connected-concepts.md` — cross-links to other topics
- `open-questions.md` — questions I still need to answer
