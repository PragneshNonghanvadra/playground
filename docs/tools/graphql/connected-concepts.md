# GraphQL — Connected Concepts

> Fill this in as related topics get studied. Don't block on it.

## Things GraphQL builds on

| Concept | Where to study later | How it connects |
|---|---|---|
| HTTP fundamentals | `docs/topics/backend-systems/` | GraphQL runs over HTTP; understanding POST semantics, headers, caching explains why GraphQL caching is hard |
| API design patterns | `docs/topics/backend-systems/` | GraphQL is one answer to API design; REST is another |
| Authentication / Authorization | `docs/topics/backend-systems/` | Field-level auth in resolvers; JWT in context; session vs token |
| N+1 and query batching | `docs/topics/data-systems/` | DataLoader is a batching pattern; the problem is a data-access pattern |
| WebSockets | `docs/topics/mobile-edge-realtime/` | Subscriptions run over WebSocket; understanding WS lifecycle explains subscription failure modes |
| Distributed caching | `docs/topics/backend-systems/` | GraphQL caching via Apollo Cache-Control, CDN integration |

## Things that build on GraphQL concepts

| Concept | Where to study later | How it connects |
|---|---|---|
| BFF (Backend for Frontend) | `docs/topics/backend-systems/` | GraphQL as a BFF layer over gRPC/REST microservices is a common pattern |
| Federation | `docs/tools/graphql/` (future file) | Apollo Federation: multiple subgraphs compose into one supergraph |
| DataLoader pattern | `docs/topics/distributed-systems/` | DataLoader is a request-scoped batching pattern — same idea as request coalescing |

## Connected tools

| Tool | Relationship |
|---|---|
| `docs/tools/rest/` | The main alternative; tradeoffs documented in `tradeoffs.md` |
| gRPC | Service-to-service alternative; often used alongside GraphQL (gRPC internally, GraphQL externally) |
| tRPC | TypeScript-specific alternative for simpler cases |
| Prisma | Common ORM used in GraphQL resolvers; its query batching complements DataLoader |

---

> Update this file when you study any of the above. Add a brief note on what the connection taught you.
