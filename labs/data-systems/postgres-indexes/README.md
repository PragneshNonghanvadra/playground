# Lab: Postgres Indexes

## Purpose

Build intuition for what indexes actually do, why query plans matter, and how the planner makes choices. Replace "add an index" hand-waving with measured behavior.

## Topic area

[`docs/topics/data-systems/`](../../../docs/topics/data-systems/)

## Learning goal

After this lab, I should be able to read an `EXPLAIN ANALYZE` plan, predict whether the planner will use a given index, and explain why a covering index can be 10x faster than a non-covering one.

## Scope

- A local Postgres (Docker) with a single table of ~10M rows.
- Variations: no index, B-tree on one column, B-tree on `(a,b)`, covering index, partial index.
- Same query repeated against each variant.
- `EXPLAIN ANALYZE` captured for each.

## Non-goals

- ORM behavior. Use raw SQL.
- Vector indexes. Separate lab.
- Replication / HA. Single-node.

## Architecture

```
[ docker postgres ] <- [ seed script (10M rows) ]
                    <- [ benchmark script ]
                    -> [ EXPLAIN ANALYZE outputs ]
```

## Implementation plan

1. Start a local Postgres in Docker.
2. Create a table representative of a real workload (e.g., `events(id, user_id, tenant_id, created_at, kind, payload)`).
3. Seed 10M rows with realistic distributions (skew on `tenant_id`, time clustering on `created_at`).
4. Run a few canonical queries: point lookup, range scan, top-N per group, "recent activity for tenant".
5. For each query, measure with no index, then with each index variant.
6. Capture and annotate `EXPLAIN (ANALYZE, BUFFERS)`.
7. Document which index the planner chose and why.

## Commands

```bash
# placeholder; flesh out during implementation
docker compose up -d postgres
psql -h localhost -U postgres -f sql/seed.sql
psql -h localhost -U postgres -f sql/benchmark.sql
```

## Expected observations

- Without an index, a 10M-row table takes seconds for a point lookup.
- A B-tree index turns it into ~1ms.
- A covering index avoids a heap fetch; meaningfully faster on selective queries.
- The planner sometimes prefers a sequential scan even when an index exists — selectivity matters.
- A partial index (e.g., `WHERE deleted_at IS NULL`) can be smaller and faster for the matching subset.

## Failure experiment

- Disable autovacuum on the table; insert/update heavily; watch query plans degrade.
- Run `ANALYZE` manually and watch plans flip.
- Build an index _concurrently_ during sustained writes; observe lock behavior.

## Debugging guide

- `EXPLAIN (ANALYZE, BUFFERS, VERBOSE)` is your main tool.
- `pg_stat_user_indexes` shows index usage in production-shaped workloads.
- `pg_stat_statements` for query-level latency.

## Benchmark plan

- Same query, 5 variants, 30s of throughput each.
- Capture: latency p50/p95/p99, plan rows estimate vs actual, buffers read.
- Compare: cold cache vs warm cache.

## Production notes

What would change to operate this:

- Online index creation: `CREATE INDEX CONCURRENTLY`. Note: doesn't lock writes, but takes longer; will fail if duplicates exist on a unique index.
- Index maintenance cost: every write updates every index. Indexes are not free.
- Index bloat over time; reindex strategies.
- Monitoring: slow query log, plan changes, statistics freshness.

## Interview notes

- Mental model of B-tree: ordered, balanced, log(n) depth.
- Why composite index `(a, b)` helps `WHERE a = ? AND b = ?` and `WHERE a = ?` but not `WHERE b = ?`.
- Why covering indexes win: avoid the heap visit.
- When a sequential scan beats an index scan: low selectivity, small table, sorted I/O.
- Why ANALYZE matters: planner relies on stats; stale stats → bad plans.

## AI-agent notes

What an AI agent might get wrong:

- "Just add an index on every column you query." Wrong: write amplification, index bloat, planner confusion.
- Forget about `EXPLAIN ANALYZE` and reason from the SQL alone.
- Suggest a hash index where a B-tree is correct, or vice versa.
- Skip `ANALYZE` after seeding and miss why the planner chose a sequential scan.

## Follow-up experiments

- GIN index for JSONB queries: compare to a normalized table.
- BRIN index for time-series append-only data.
- Multi-column index ordering: which column first matters and why.
- Same workload on SQLite vs Postgres: compare plan quality.

## Status

- [ ] Implementation pending
- [ ] First measurement taken
- [ ] Failure experiment run
- [ ] Lessons written back to `docs/topics/data-systems/`
