# GraphQL — Experiment Ideas

Lab lives at: [`labs/tools/graphql-fundamentals/`](../../../labs/tools/graphql-fundamentals/)

## Experiment 1 (start here): Schema + resolver from scratch

**Teaches**: What GraphQL actually does under the hood, before any framework magic.

Build a GraphQL server using only the `graphql` npm package (the reference implementation). No Apollo, no Yoga.

- Write a schema in SDL.
- Write resolvers by hand.
- Execute queries directly via `graphql(schema, query, rootValue, context)`.
- Build a tiny HTTP handler that calls `graphql()` on each POST.

You'll see: the parse/validate/execute steps explicitly. Nothing hidden.

**Measurement**: None required. Observation: confirm the 200-OK-with-errors behavior. Confirm invalid queries are rejected before execution.

---

## Experiment 2: N+1 in action (then DataLoader)

**Teaches**: Why N+1 happens and exactly how DataLoader fixes it.

1. Build: `Query.posts → Post.author` with a naive per-item DB call.
2. Log every DB query.
3. Run a query for 20 posts. Count the queries. Observe 21.
4. Add DataLoader. Run the same query. Count the queries. Observe 2.
5. Intentionally share a DataLoader across requests (a bug). Run two concurrent requests. Observe the data cross-contamination.

**Measurement**: DB query count per request. p50 latency before and after DataLoader.

**Failure experiment**: the shared-DataLoader bug — demonstrate data leakage.

---

## Experiment 3: Auth at root vs auth at field level

**Teaches**: The field-level auth gap.

1. Build a schema with `User` type including `email` and `phone` fields marked sensitive.
2. Add auth only to `Query.userById`.
3. Add `Post.author: User` without auth.
4. Query `post(id: "public") { author { email } }` without auth credentials.
5. Observe you can read `email` despite the root auth check.
6. Fix by adding field-level auth to `User.email` and `User.phone`.

**Measurement**: N/A. Observation-based.

**Failure experiment**: this IS the failure experiment.

---

## Experiment 4: Query depth / complexity attack

**Teaches**: What unconstrained queries do to a server.

1. Build a schema with circular potential: `Post.author → User.posts → Post.author...`
2. Send a query 10 levels deep.
3. Observe memory/CPU.
4. Add `graphql-depth-limit(5)`.
5. Observe rejection before execution.

**Measurement**: CPU and memory at increasing depth levels.

---

## Experiment 5: Persisted queries

**Teaches**: What persisted queries solve and the client/server handshake.

1. Implement APQ (Automatic Persisted Queries) with Apollo Server.
2. On first request: client sends hash only → server says "unknown, send full query" → client resends full query.
3. On subsequent requests: client sends hash only → server responds from cache.
4. Measure payload size reduction.

**Measurement**: request payload bytes, round-trip latency (first vs subsequent).

---

## Experiment 6: Subscription under load

**Teaches**: What stateful WebSocket connections look like at scale.

1. Start a subscription server with in-process PubSub.
2. Open 50 concurrent WebSocket connections.
3. Publish 100 events.
4. Observe delivery latency and connection count.
5. Kill a client without clean disconnect. Observe whether the server reclaims the connection.

**Measurement**: event delivery latency, memory growth per connection.

**Failure experiment**: connection leak when client disconnects without close handshake.

---

## Experiment 7: codegen end-to-end

**Teaches**: How codegen bridges schema and TypeScript types.

1. Write an SDL schema.
2. Set up `graphql-codegen` with the `typescript` + `typescript-operations` + `typescript-react-apollo` plugins.
3. Write a client query.
4. Run codegen. Observe the generated types.
5. Change a field name in the schema without updating the operation.
6. Run codegen. Observe the TypeScript error.
7. Set up the codegen check in CI (`graphql-codegen --check`).

**Measurement**: N/A. Observation of compile-time vs runtime error timing.

---

## Suggested first lab to start

**Experiment 2** (N+1 and DataLoader). It's the highest-leverage thing to understand about GraphQL in practice, and you can measure it concretely. It also forces you to understand context and resolvers properly, which unblocks experiments 3 and 4.
