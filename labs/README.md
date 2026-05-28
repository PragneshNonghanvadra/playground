# Labs

Tiny, focused experiments grouped by topic. Each lab tests **one** concept.

## How to add a lab

1. Pick the matching topic folder under `labs/<topic>/`.
2. Create a new subfolder for your lab.
3. Use [`templates/lab-template.md`](../templates/lab-template.md) as the lab `README.md`.
4. Keep scope small. A lab should fit in your head.
5. Always include at least one **measurement** and at least one **failure experiment**.
6. Write lessons back to `docs/topics/<topic>/`.

## Conventions

- Each topic folder has `experiments/`, `notes/`, `benchmarks/`, `failure-experiments/` for accumulated work that doesn't need its own lab folder.
- Labs that produce reusable code may eventually graduate to `packages/` (with an ADR explaining why).

## Initial labs

- [`backend-systems/http-api-basics/`](backend-systems/http-api-basics/) — HTTP from first principles, idempotency, rate limiting.
- [`data-systems/postgres-indexes/`](data-systems/postgres-indexes/) — B-trees, query plans, covering indexes, online migrations.
- [`distributed-systems/queues-and-idempotency/`](distributed-systems/queues-and-idempotency/) — at-least-once delivery, idempotent consumers, DLQs.
- [`ai-engineering/rag-from-scratch/`](ai-engineering/rag-from-scratch/) — chunking, embeddings, retrieval, grounding, evals.
- [`observability/logs-metrics-traces/`](observability/logs-metrics-traces/) — RED metrics, structured logs, OpenTelemetry traces.

## Quality bar

A lab is "done" when:

- The README follows the lab template.
- A measurement is captured.
- A failure experiment is documented.
- Lessons are written back to the topic notes.
