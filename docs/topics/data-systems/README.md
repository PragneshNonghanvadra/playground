# Data Systems

## Why this topic matters

Data systems are where **time-correctness happens**. Almost every "weird" backend bug in production traces back to a data-system question: an isolation level you didn't realize you had, an index that wasn't used, a migration that didn't lock the way you thought, replication lag that made a feature lie to a user.

Data systems are also where **leverage compounds**: the right index turns a 30-second query into 30ms; the wrong schema makes every feature 2x harder for years.

For an AI-native engineer, data systems extend into **vector indexes, hybrid retrieval, and ACL-aware search**. RAG pipelines depend on the same fundamentals: schemas, indexes, query plans, and consistency.

## Subtopics inside this area

- **SQL** — relational model, joins, window functions, CTEs, subqueries; when SQL beats application code.
- **Indexes** — B-tree, hash, GIN, GiST, BRIN, vector (HNSW, IVF). When each shines, what they cost on write.
- **Query plans** — `EXPLAIN ANALYZE`; planner statistics; when the planner gets it wrong; plan stability.
- **Transactions** — ACID; what each letter actually guarantees; what they cost.
- **Isolation levels** — read uncommitted → serializable; the anomalies each prevents and allows; phantom reads, write skew.
- **Migrations** — online schema changes; locking gotchas; backfills; reversibility.
- **Replication** — sync vs async; logical vs physical; read replicas and replication lag.
- **Backups** — point-in-time recovery; the only backup that matters is the one you've restored.
- **Schema design** — normalization vs denormalization; when to denormalize on purpose.
- **Caching the database** — read-through caches; invalidation; the hardest problem.
- **OLTP vs OLAP** — when to split; CDC into a warehouse; columnar formats (Parquet, Arrow).
- **Time-series & event data** — append-only models; TTLs; downsampling.
- **Vector data** — embeddings as columns; HNSW/IVF; hybrid search with filters.
- **Data integrity** — constraints, foreign keys, check constraints; why "we'll enforce in the app" usually fails.
- **Multi-tenant data** — column-, schema-, or db-per-tenant; index implications.

## How this connects to other topics

| Connects to             | How                                                                                  |
| ----------------------- | ------------------------------------------------------------------------------------ |
| **Backend systems**     | Every API endpoint is, at heart, a database query.                                   |
| **Distributed systems** | Replication, isolation, and consensus all live here.                                 |
| **Performance**         | The database is almost always the bottleneck (or the cause of one).                  |
| **AI engineering**      | Embeddings, vector indexes, hybrid retrieval, RAG ingestion — all data systems work. |
| **Observability**       | Slow query logs, plan changes, lock waits are observability concerns.                |
| **Security**            | Row-level security, column-level encryption, PII isolation.                          |
| **Product engineering** | Activation queries, funnels, retention analyses are SQL skills.                      |

## Suggested first experiments

1. **`labs/data-systems/postgres-indexes`** — Build a small Postgres dataset (10M+ rows). Run a query without an index. With a B-tree. With a covering index. With and without statistics updated. Inspect plans. Measure latency.
2. **Isolation level demo** — Reproduce write skew under READ COMMITTED; show how SERIALIZABLE prevents it; measure the cost.
3. **Online migration on a large table** — `ALTER TABLE ... ADD COLUMN` with a non-null default. Show the gotcha. Then do it the safe way (nullable add, backfill, set not null in batches).
4. **Read-replica lag UX** — Write to primary, read from replica. Demonstrate user-visible "I just saved this — where is it?" Add read-your-writes routing.
5. **Vector vs full-text vs hybrid** — Same corpus, three retrieval strategies. Measure precision and latency.

## Interview relevance

Universally hot at senior IC level:

- "How does a B-tree index work?" (mental model, not implementation details)
- "What's the difference between READ COMMITTED and REPEATABLE READ?" (and which one your DB defaults to)
- "How would you migrate this column without downtime?" (online migrations)
- "When would you choose Postgres over DynamoDB / Cassandra?" (access patterns, transactions, scale shape)
- "How do you handle replication lag?" (read-your-writes, sticky sessions, primary reads)
- "What's the difference between OLTP and OLAP?" (and when to split)
- "How would you implement vector search at scale?" (HNSW vs IVF, hybrid filters, refresh strategy)

Strong answers reach for **access patterns** before naming a tool, mention **isolation levels** by name, and explicitly handle **migrations and backups**.

## Topic notes in this folder

- `mental-model.md`, `why-it-matters.md`, `must-know.md`, `good-to-know.md`, `beginner-mistakes.md`, `senior-engineer-thinking.md`, `ai-native-perspective.md`, `production-concerns.md`, `failure-modes.md`, `scaling-considerations.md`, `tradeoffs.md`, `interview-notes.md`, `resources.md`, `experiments.md`, `connected-concepts.md`, `open-questions.md`.
