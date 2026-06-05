# Tool: GraphQL

## Status

- **Stage**: In progress
- **Confidence**: shallow → working
- **Last updated**: 2026-05-28
- **Lab**: [`labs/tools/graphql-fundamentals/`](../../../labs/tools/graphql-fundamentals/)

## What is it

GraphQL is a **query language for APIs** and a **runtime for executing those queries**.

The client specifies exactly what data it needs. The server returns exactly that — no more, no less. A single endpoint (`POST /graphql`) serves all operations.

It was created at Facebook in 2012, open-sourced in 2015. The core insight: mobile apps needed fine-grained control over what data was fetched, because bandwidth and battery mattered.

## What problem it actually solves

REST puts the server in charge of response shape. The server decides what a `GET /posts/:id` returns. If a mobile client only needs `title` and `thumbnail`, it still gets `body`, `author`, `tags`, and `comments`.

GraphQL flips that: **the client is in charge of the response shape**. The server publishes a typed schema describing what's *possible*; the client describes what it *wants* from that schema.

This matters most when:

- Multiple clients (web, mobile, partner API) need different slices of the same data.
- The frontend team and backend team move at different speeds.
- You want a single, typed, self-documenting API surface that clients can introspect.

## When to reach for it

- You have **multiple client types** with meaningfully different data needs.
- You have **nested / relational data** that REST would force into N requests or messy `?include=` params.
- You want a **typed contract** that clients can introspect and tooling can validate.
- You're building a **product API** (consumed by your own apps), not a public API meant for external developers who don't control the client.

## When NOT to reach for it

- **Simple CRUD with one client** — REST or tRPC is less ceremony.
- **Public / open API** — REST is more universally understood, and GraphQL exposes your schema to anyone who queries it.
- **File uploads** — doable but awkward in GraphQL.
- **Caching is critical** — REST's GET requests cache at the HTTP layer for free; GraphQL uses POST, which doesn't.
- **Your team isn't ready for resolver patterns** — GraphQL has a learning curve; it's easy to introduce N+1 problems without knowing it.

## Files in this folder

| File | What it covers |
|---|---|
| [`mental-model.md`](mental-model.md) | The graph, schema-as-contract, resolvers as functions |
| [`core-concepts.md`](core-concepts.md) | Schema, types, queries, mutations, subscriptions, fragments, directives, introspection |
| [`resolver-patterns.md`](resolver-patterns.md) | Resolver chain, context, N+1, DataLoader, auth in resolvers |
| [`server-and-client.md`](server-and-client.md) | Apollo Server/Client, Yoga, Pothos, urql, code-first vs schema-first |
| [`production-concerns.md`](production-concerns.md) | Auth, N+1, rate limiting, depth/complexity limiting, persisted queries |
| [`failure-modes.md`](failure-modes.md) | N+1 hammering DB, auth bypass, circular queries, subscription leaks |
| [`tradeoffs.md`](tradeoffs.md) | GraphQL vs REST vs gRPC vs tRPC |
| [`interview-notes.md`](interview-notes.md) | 30s / 2m / deep-dive framings |
| [`resources.md`](resources.md) | Curated references |
| [`experiments.md`](experiments.md) | Lab ideas |
| [`connected-concepts.md`](connected-concepts.md) | Links to topic notes (fill in as topics get studied) |
