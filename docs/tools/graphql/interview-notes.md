# GraphQL — Interview Notes

## 30-second explanation

GraphQL is a query language where the client specifies exactly what data it needs from a typed schema the server publishes. A single endpoint handles all operations — queries, mutations, and subscriptions. The key advantages are no over/under-fetching, a self-documenting typed contract, and the ability to traverse relational data in one request.

## 2-minute explanation

REST puts the server in charge of response shape. Every endpoint returns a fixed structure regardless of what the client actually needs. This causes over-fetching (mobile gets desktop-sized payloads) and under-fetching (a page needs 3 round-trips to assemble its data).

GraphQL flips the model: the server publishes a type system (schema) describing everything available. The client writes a query document declaring exactly what it wants. The runtime validates the query against the schema, executes resolvers for each requested field, and returns exactly that shape.

The critical implementation concern is the N+1 problem: resolvers are called per-field per-object, so a query for 100 posts triggers 100 individual author lookups unless you use DataLoader to batch them. Auth must be enforced at the field level, not just at the entry point — because nested paths can reach sensitive data from an otherwise-public starting point.

Production GraphQL requires: DataLoader for batching, depth/complexity limits against query exhaustion, introspection disabled or restricted, field-level auth, and monitoring that tracks GraphQL errors not just HTTP status codes.

## Deep-dive explanation

Cover in order:
1. The mental model: schema as contract, resolver as function, runtime composes.
2. The query lifecycle: parse → validate → execute.
3. The three root types: Query (parallel), Mutation (sequential), Subscription (WebSocket).
4. Fragment co-location: components own their data requirements.
5. N+1 and DataLoader (batch per request tick, created per-request never shared).
6. Auth: field-level enforcement, not root-only.
7. Tradeoffs vs REST (caching, complexity, over-fetching).
8. Production: depth limits, complexity limits, introspection, persisted queries, codegen, 200-OK error monitoring.
9. Alternatives: when tRPC is simpler, when gRPC is more appropriate.

## Common interview questions

- What is GraphQL and what problem does it solve?
- Explain the N+1 problem and how DataLoader fixes it.
- How is authorization handled in GraphQL? What's the most common mistake?
- Why doesn't GraphQL use HTTP caching the same way REST does?
- What is query depth limiting and why is it necessary?
- What are persisted queries and when would you use them?
- When would you choose REST over GraphQL?
- What is schema-first vs code-first and what are the tradeoffs?
- Explain how subscriptions work and what's needed to scale them.
- What's introspection and should you expose it in production?

## Strong answer patterns

Strong answers always include:
- **N+1 mentioned proactively**, not just when prompted.
- **Auth concern at field level**, not just "you check a JWT".
- **Caching as a real tradeoff**, not glossed over.
- **Comparison to REST**: when GraphQL wins, and when it doesn't.
- **Monitoring gap**: HTTP 200 ≠ success in GraphQL.
- **DataLoader as the N+1 solution** (not "add an index", not "write a JOIN resolver").

## Weak answer patterns

- "GraphQL is better than REST because it's faster." (Not inherently true; N+1 can make it slower.)
- Naming the tools (Apollo, urql) without explaining what GraphQL itself does.
- Forgetting that mutations run sequentially.
- Treating introspection as just a dev convenience, not a security surface.
- Not knowing what DataLoader does internally (batching per event loop tick).

## Diagrams worth drawing

1. **The resolver chain**: `Query.posts → Post.author (×N) → DataLoader → single batch query`.
2. **Schema → operations**: show that the client writes a document against a server-defined schema.
3. **HTTP 200 with errors envelope**: `{ data: { post: null }, errors: [...] }`.

## Tradeoffs interviewers want explicit mention of

- Client controls shape ↔ server can't optimize response layout.
- One endpoint ↔ no HTTP-layer caching.
- Type-safe contract ↔ codegen adds toolchain complexity.
- Expressive queries ↔ N+1 risk and complexity attacks.
- Single schema ↔ every auth change must be propagated per-field.

## Related system design problems

- "Design a social feed API serving web, iOS, and Android." (GraphQL fits well: different clients, different data needs.)
- "Design a product catalog API for a marketplace." (REST vs GraphQL tradeoff depends on consumer diversity.)
- "How would you scale a real-time chat?" (Subscription scaling, WebSocket statefulness, shared PubSub.)
- "Design a BFF (backend for frontend) layer." (GraphQL as a BFF over REST/gRPC microservices is a common pattern.)
