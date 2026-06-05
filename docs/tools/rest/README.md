# Tool: REST

> Quick reference — you already know this. Kept as a baseline to compare against GraphQL, gRPC, tRPC.

## What it is

REST (Representational State Transfer) is an architectural style where resources are identified by URLs and manipulated via HTTP verbs. Every endpoint represents a noun; every HTTP method represents an action on it.

## The six constraints that make something "RESTful"

1. **Client-server** — UI and backend are separate.
2. **Stateless** — every request carries all the context needed.
3. **Cacheable** — responses declare whether they're cacheable.
4. **Uniform interface** — resources, representations, self-descriptive messages, HATEOAS.
5. **Layered system** — client can't tell if it's talking to origin or a proxy.
6. **Code on demand** (optional) — server can send executable code.

In practice, most APIs are "REST-ish" — they use HTTP + JSON but ignore caching headers and HATEOAS.

## The idiomatic shape

```
GET    /posts          - list
GET    /posts/:id      - read one
POST   /posts          - create
PUT    /posts/:id      - replace (idempotent)
PATCH  /posts/:id      - partial update
DELETE /posts/:id      - delete (idempotent)
```

## What REST is good at

- **Cache-friendly** — GET responses are cacheable at HTTP layer (CDN, browser, proxy).
- **Simple mental model** — URL = resource, verb = action.
- **Huge tooling ecosystem** — cURL, Postman, OpenAPI, every HTTP client.
- **Easy to reason about in logs and monitoring** — method + path + status is enough to understand a request.
- **Firewall / proxy friendly** — HTTP is universally understood.

## What REST struggles with

- **Over-fetching** — endpoint returns more fields than the client needs.
- **Under-fetching** — client needs data from multiple endpoints per page render.
- **Versioning** — `/v1/posts` vs `/v2/posts` creates drift and duplication.
- **Strongly typed contracts** — OpenAPI helps but is a separate tooling layer, not intrinsic.
- **Real-time** — not built-in; requires SSE or WebSockets as an add-on.
- **Nested / relational data** — requires N requests or bespoke `?include=author,comments` patterns.

## Key production concerns

- Idempotency keys on POST (not built-in; must be designed explicitly).
- Versioning strategy: URL versioning (`/v2/`) vs header versioning (`Accept: application/vnd.api+json;version=2`).
- Pagination: offset vs keyset — know the difference and default to keyset for large datasets.
- Error shape: be consistent. Pick one: RFC 7807 Problem JSON or your own; stick to it.
- Authentication: header (`Authorization: Bearer ...`) is standard; cookie auth adds CSRF surface.

## Connected concepts (to link later)

- `docs/topics/backend-systems/` — API design, idempotency, auth
- `docs/tools/graphql/tradeoffs.md` — side-by-side comparison
