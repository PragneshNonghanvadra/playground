# Lab: GraphQL Fundamentals

## Purpose

Work through the core mechanics of GraphQL hands-on: schema + resolvers from scratch, the N+1 problem, DataLoader batching, and field-level auth.

## Topic area

[`docs/tools/graphql/`](../../../docs/tools/graphql/)

## Learning goal

After this lab, I should be able to:
- Write a GraphQL schema and resolvers without relying on framework magic.
- Observe and measure the N+1 problem with my own eyes.
- Apply DataLoader correctly (per-request, not shared).
- Demonstrate the field-level auth gap.
- Add depth limiting and show a malicious query get rejected.

## Scope

- Node.js + the `graphql` npm package (reference implementation) for Experiment 1.
- Node.js + Apollo Server or Yoga for Experiments 2–4.
- SQLite or an in-memory store (no Postgres required yet).
- No frontend — `curl` or Apollo Sandbox is the client.

## Non-goals

- Production deployment.
- Subscriptions (separate experiment once fundamentals are solid).
- Codegen setup (separate experiment).
- Federation.

## Experiment sequence

Work through these in order. Each builds on the previous.

### Experiment 1: Schema + resolver from scratch

Goal: understand parse → validate → execute before any framework.

```
src/
  schema.ts     # SDL string
  resolvers.ts  # resolver map
  server.ts     # tiny HTTP handler calling graphql()
  data.ts       # in-memory "database"
```

### Experiment 2: N+1 and DataLoader

Goal: observe and measure.

```
src/
  db.ts         # simulate a DB with configurable latency + query log
  resolvers.ts  # naive resolvers first, then DataLoader version
  queries/      # the problematic query to run
  benchmark.ts  # count queries per request
```

### Experiment 3: Auth gap

Goal: demonstrate field-level exposure without field-level auth.

```
src/
  schema.ts     # Post.author: User, User.email is "sensitive"
  auth.ts       # JWT or simple token check
  resolvers.ts  # auth at root only (broken), then auth at field (fixed)
```

### Experiment 4: Depth + complexity limits

Goal: show server behaviour under adversarial queries.

```
src/
  validation.ts # depth-limit and complexity-limit configuration
  queries/
    shallow.graphql    # normal query
    deep-attack.graphql  # deeply nested circular query
```

## Commands

```bash
# Install (once implemented)
pnpm install

# Run the server
pnpm dev

# Run a query via curl
curl -X POST http://localhost:4000/graphql \
  -H 'Content-Type: application/json' \
  -d '{"query": "{ posts { title author { name } } }"}'

# Run benchmarks
pnpm benchmark

# Run failure experiments
pnpm run experiment:auth-gap
pnpm run experiment:depth-attack
```

## Expected observations

- Experiment 1: direct `graphql()` call returns exactly what the query asked for. Invalid queries are rejected before any resolver runs.
- Experiment 2: 20 posts = 21 queries without DataLoader; 2 queries with DataLoader. Latency drops proportionally.
- Experiment 3: `post { author { email } }` returns email without auth when field-level check is missing.
- Experiment 4: deeply nested query causes memory spike; rejected immediately with depth limit configured.

## Failure experiments

1. **N+1 in prod shape**: send the naive 100-post query to a slow mock DB. Watch p99 time out.
2. **Shared DataLoader (data leak)**: create DataLoader at module scope; run two concurrent requests; observe cross-contamination.
3. **Field-level auth bypass**: query a sensitive field through a nested path.
4. **Depth attack**: 15-level nested circular query with no depth limit.

## Benchmark plan

- N+1 experiment: 1 request × [5, 20, 50, 100] posts. Measure DB query count and total latency.
- DataLoader: same workload. Measure query count and latency again. Plot comparison.

## Production notes

Before using this in production:

- Add persisted queries (APQ).
- Add `graphql-armor` for depth + complexity + field suggestion disabling.
- Disable introspection in production.
- Replace in-memory data with a real DB + DataLoader.
- Codegen on every schema change.
- Monitor GraphQL `errors` array, not just HTTP status.

## Interview notes

After this lab: "Explain the N+1 problem in GraphQL" becomes a 2-minute answer with a concrete story — because you've measured it yourself.

## AI-agent notes

Common agent mistakes on GraphQL labs:
- Scaffold with Apollo Server before showing the bare `graphql` call. Hides what the runtime actually does.
- Create DataLoader at module scope (a real bug — causes data leakage).
- Skip the `context` pattern and put the DB client in a global variable.
- Generate resolvers that return the correct data but ignore the N+1 problem.

## Follow-up labs

- `graphql-subscriptions/` — real-time with WebSocket, shared PubSub, connection lifecycle.
- `graphql-codegen/` — end-to-end type safety with schema codegen.
- `graphql-vs-rest/` — same resource implemented in both; compare payload, request count, latency.
- `graphql-federation/` — when and how to split a schema across multiple subgraphs.

## Status

- [ ] Experiment 1 implemented
- [ ] Experiment 2 implemented + measured
- [ ] Experiment 3 implemented (failure experiment)
- [ ] Experiment 4 implemented + measured
- [ ] Lessons written back to `docs/tools/graphql/`
