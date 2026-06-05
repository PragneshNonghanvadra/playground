# GraphQL — Failure Modes

## FM-1: N+1 silently hammer the database

**What happens**: A query for 100 posts fires 101+ individual DB queries. p50 latency looks fine. p99 collapses under moderate load. DBA starts seeing "max connections exceeded".

**Trigger**: Any resolver that makes per-item DB calls (`Post.author: () => db.findUser(parent.authorId)`) without DataLoader.

**Symptoms**: DB query volume grows linearly with list size. Slow logs full of `SELECT ... WHERE id = ?` single-row lookups. Latency spikes on paginated queries.

**Detection lag**: Often invisible until the first load test or a user triggers a large list.

**Mitigation**:
- DataLoader on every resolver that loads by a parent ID.
- Log DB query count per request in development (Prisma's `queryLogger`, Knex debug mode).
- Set up a query count circuit breaker in test: if a single request fires >100 queries, the test fails.

---

## FM-2: Auth bypass via nested resolvers

**What happens**: A developer adds auth to `Query.userById` but forgets that `Post.author` also returns a User. Attacker queries `post(id: "public-post") { author { email, phone } }` and gets full user details.

**Trigger**: Auth checked only at root Query level, not at field-resolver level for sensitive types.

**Symptoms**: No obvious error. No audit log hit. Silent data exposure.

**Detection lag**: Only discovered via manual security review or external pen test.

**Mitigation**:
- Auth checks at the *field* level for sensitive types, not just the root query.
- Schema directive (`@auth`) enforced at the schema level as a default-deny.
- Security review: for every type with sensitive fields, trace every path in the schema that can reach it.

---

## FM-3: Circular / deeply nested query exhaustion

**What happens**: Client (accidental or malicious) sends a deeply nested query. Server OOMs or CPU-saturates before returning.

**Trigger**: No depth limit configured. Schema has circular references (`Post.author.posts.author.posts...`).

**Symptoms**: Memory spike. CPU spike. Server goes OOM or becomes unresponsive. GraphQL error doesn't appear — the server dies.

**Mitigation**: `graphql-depth-limit`. `graphql-validation-complexity`. Both.

---

## FM-4: Schema drift between codegen and deployed schema

**What happens**: A developer changes a field name in the schema but doesn't regenerate types. TypeScript still compiles (old type cached). Production resolver returns `undefined` for the renamed field. Client gets null where it expected data.

**Trigger**: Codegen not run as part of the CI pipeline. Manually running codegen is forgotten.

**Symptoms**: `null` values in client for fields that should have data. No type error at compile time.

**Mitigation**: Run `graphql-codegen` in CI. Use `graphql-inspector` to detect breaking changes as a CI check.

---

## FM-5: "HTTP 200 with errors" swallowed by monitoring

**What happens**: GraphQL returns HTTP 200 with `{ "errors": [...] }`. Your HTTP-level error rate monitor stays at 0%. Users are experiencing errors. Nobody notices for hours.

**Trigger**: Monitoring only tracks HTTP status codes, not GraphQL response body.

**Detection lag**: Until a user reports it or you happen to look at the response body.

**Mitigation**:
- Parse the GraphQL response body in your monitoring.
- Track `errors` array presence as a separate error metric.
- Apollo Studio and most observability plugins handle this automatically.
- Alert on GraphQL error rate, not just HTTP 5xx rate.

---

## FM-6: DataLoader shared across requests

**What happens**: A DataLoader instance is created once at module load time and shared across all requests. User A's request populates the loader cache. User B's request reads from that cache and sees User A's data.

**Trigger**: `const userLoader = new DataLoader(...)` at module scope instead of inside the context factory.

**Symptoms**: Data leakage between users. Intermittent incorrect data. Extremely difficult to reproduce in testing (depends on request ordering and cache TTL).

**Detection lag**: Very high — only visible under concurrent load, and symptoms look like "wrong data sometimes".

**Mitigation**: Always create DataLoader instances inside the context factory. Never share them.

---

## FM-7: Subscription connection leak

**What happens**: Clients connect for subscriptions, disconnect without clean shutdown, and the server never reclaims resources. Memory and connection count grows over hours/days.

**Trigger**: No `onDisconnect` cleanup. No idle timeout on WebSocket connections. No monitoring on active connection count.

**Symptoms**: Memory grows steadily over time. WebSocket connection count grows. Server eventually crashes or becomes unresponsive.

**Mitigation**: Implement `onDisconnect` to clean up subscriptions. Set WebSocket idle timeouts. Monitor active connection count. Load test subscriptions with disconnection scenarios.

---

## FM-8: Full query parsing on every request (missing persisted queries)

**What happens**: Every request parses and validates the full query string. Under high load, parsing and validation become a non-trivial CPU cost.

**Trigger**: Persisted queries not enabled. Large, complex queries sent as strings on every request.

**Symptoms**: Slightly elevated CPU per request. Under load, CPU-bound before DB-bound. Latency grows sub-linearly with traffic.

**Mitigation**: Enable Automatic Persisted Queries (APQ). Cache parsed query ASTs.

---

## FM-9: Mutations run sequentially — unintended ordering issues

**What happens**: A client sends multiple mutations in one request. GraphQL executes mutations sequentially (by spec). The order may not match what the developer assumed.

```graphql
mutation {
  deletePost(id: "1")    # runs first
  createPost(input: ...) # runs second
}
```

**Trigger**: Multiple mutations in one document, order assumptions not explicit.

**Mitigation**: Be explicit. Send one mutation per request unless you fully understand the execution order. Document this for your team.
