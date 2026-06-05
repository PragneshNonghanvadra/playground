# GraphQL — Tradeoffs: GraphQL vs REST vs gRPC vs tRPC

## Quick summary

| Axis | GraphQL | REST | gRPC | tRPC |
|---|---|---|---|---|
| Client controls shape | Yes | No | No | Yes (TypeScript) |
| HTTP caching | Hard | Easy (GET) | No | Varies |
| Type safety | Via codegen | Via OpenAPI codegen | Native (protobuf) | Native (TypeScript) |
| Learning curve | High | Low | High | Low (if TS) |
| Browser-friendly | Yes | Yes | No (needs grpc-web) | Yes |
| Real-time | Subscriptions (WebSocket) | Add-on (SSE/WS) | Streaming RPCs | Add-on |
| Schema discovery | Introspection | OpenAPI | Reflection | TypeScript inference |
| Public API fit | Okay | Best | Poor | Poor (TS-specific) |
| File handling | Awkward | Easy | Poor | Varies |
| Ecosystem maturity | Very mature | Extremely mature | Mature | Growing (2022+) |

---

## GraphQL vs REST

**Choose GraphQL when**:
- Multiple clients (web, mobile, partner) need different data shapes from the same domain.
- Your data is naturally relational/nested and REST would force N requests per page.
- You want a typed, self-documenting, introspectable contract.
- The frontend team moves faster than the backend and needs to fetch exactly what it needs without blocking on endpoint changes.

**Choose REST when**:
- Simple CRUD, one primary client.
- Caching is critical (public content, CDN-cacheable responses).
- Public API for external developers who don't know GraphQL.
- File uploads are a first-class concern.
- Team is small and GraphQL's overhead isn't worth it.

**The honest hidden cost of GraphQL**:
- N+1 if DataLoader isn't wired up.
- Auth must be enforced at every field, not just every endpoint.
- Caching requires deliberate design.
- Tooling (codegen, schema management) is real ongoing maintenance.
- Your bundle grows (Apollo Client ~40KB).

---

## GraphQL vs gRPC

**gRPC** uses Protocol Buffers (binary encoding) and HTTP/2. It's designed for service-to-service (server-to-server) communication where performance matters.

**Choose gRPC when**:
- Service-to-service calls in a backend (not browser clients).
- High-throughput, low-latency RPC (binary encoding wins).
- Polyglot services (protobuf generates clients in many languages).
- Strong schema contracts and forward/backward compatibility via proto evolution.

**Choose GraphQL when**:
- Browser clients consume the API.
- Client data requirements vary widely.
- Developer ergonomics matter more than raw throughput.

They're not competing in the same niche: gRPC is for the internal service mesh; GraphQL is for the external product API. Many systems use both.

---

## GraphQL vs tRPC

**tRPC** gives you end-to-end type safety if client and server are both TypeScript, without a codegen step or query language.

```ts
// Server
const appRouter = router({
  post: {
    byId: publicProcedure.input(z.string()).query(({ input }) => db.post.findUnique({ where: { id: input } })),
  },
});

// Client — TypeScript just knows the return type
const post = await trpc.post.byId.query('123');
```

**Choose tRPC when**:
- Full-stack TypeScript (Next.js monorepo, T3 stack).
- Internal API consumed only by your own TypeScript clients.
- You want type safety without codegen ceremony.
- Simple request/response patterns (tRPC doesn't have the query-shaping power of GraphQL).

**Choose GraphQL when**:
- Multiple client types (non-TypeScript clients, mobile, partners).
- Clients need to request different fields from the same type.
- You want introspection and a schema that exists independently of the codebase.

**The core tradeoff**: tRPC is simpler when you control both ends and they're both TypeScript. GraphQL is more powerful when clients are diverse or the data model is complex.

---

## Reversal triggers (when to switch away from GraphQL)

- Caching becomes a critical performance concern and GraphQL caching complexity is too high → consider REST for cacheable resources alongside GraphQL for interactive data.
- The API becomes a public/external API with broad consumer diversity → consider REST for the public surface.
- The team is consistently getting N+1 and auth bugs despite tooling → the operational overhead may not be worth it for this team's maturity.
- Client types become homogeneous (single TypeScript frontend) → tRPC is simpler.

---

## Choosing in practice

For a **product with a web app + mobile app + potential third-party integrations**: GraphQL is well-suited.

For a **simple web app, one team, all TypeScript**: tRPC is less ceremony.

For a **public API consumed by many developers**: REST (GraphQL requires clients to understand the query language and toolchain).

For **internal service communication**: gRPC.
