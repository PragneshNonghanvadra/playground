# GraphQL — Production Concerns

## 1. The N+1 problem is the first thing that kills you

Already covered in `resolver-patterns.md`, but worth repeating as the number-one production concern.

A single query can fire thousands of DB queries if DataLoader is not wired up. This isn't a hypothetical — it will happen in your first non-trivial load test.

**Checklist before going to production**:
- [ ] Every resolver that loads by a parent ID uses DataLoader, not a direct DB call.
- [ ] DataLoader instances are created per request (not shared across requests).
- [ ] Load test the endpoint with realistic nesting depth before launch.

## 2. Authentication and authorization

GraphQL doesn't enforce auth at the transport layer by default. You own it.

**Authentication** (is the user who they say they are?): typically handled in the context factory, before resolvers run.

```ts
context: async ({ req }) => {
  const user = await verifyJWT(req.headers.authorization);
  if (!user) return { user: null, db };
  return { user, db };
}
```

**Authorization** (is the user allowed to do this specific thing?): must be enforced in every resolver that touches sensitive data. If you forget one resolver, it's exposed.

Common mistakes:
- Checking auth in the `Query.posts` resolver but forgetting it in `User.posts`. A user can fetch others' posts by querying via a `User` type.
- Trusting that "the client won't ask for it" — introspection reveals the schema; anyone can craft a query for any field.
- Checking top-level fields but not nested ones.

**Field-level authorization**: if a field should only be visible to certain roles, it must be checked in that field's resolver, not just at the root query level.

## 3. Query depth and complexity limiting

GraphQL allows arbitrarily nested queries. Without limits, a malicious or buggy client can craft a deeply nested circular query:

```graphql
{
  posts {
    author {
      posts {
        author {
          posts { ... }  # arbitrarily deep
        }
      }
    }
  }
}
```

This can exhaust memory and CPU before a database query is even made.

**Depth limiting**: reject queries beyond a max depth (commonly 5–7).

```ts
import depthLimit from 'graphql-depth-limit';

const server = new ApolloServer({
  validationRules: [depthLimit(7)],
});
```

**Complexity limiting**: assign a cost to each field and reject queries above a total cost.

```ts
import { createComplexityLimitRule } from 'graphql-validation-complexity';

const server = new ApolloServer({
  validationRules: [createComplexityLimitRule(1000)],
});
```

Use both in production.

## 4. Introspection in production

Introspection is a feature — and an attack surface. It reveals your full schema to anyone who can reach the endpoint.

Options:
1. **Disable entirely** (strictest): reject all introspection queries.
2. **Restrict to authenticated users**: only allow introspection with a valid auth token.
3. **Allow but monitor**: track who's introspecting and how often.

```ts
const server = new ApolloServer({
  introspection: process.env.NODE_ENV !== 'production',
});
```

Most public APIs disable introspection. Internal APIs may allow it for developers.

## 5. Error exposure

By default, many servers leak internal error messages to clients. In production, mask unexpected errors:

```ts
const server = new ApolloServer({
  formatError: (formattedError, error) => {
    // Log the full error internally
    console.error(error);

    // Mask internal errors for clients
    if (!(error instanceof GraphQLError)) {
      return { message: 'Internal server error', extensions: { code: 'INTERNAL_ERROR' } };
    }
    return formattedError;
  },
});
```

The distinction: errors you throw explicitly (GraphQLError with `code: 'NOT_FOUND'`) can surface to clients. Unexpected errors from your database or runtime should not.

## 6. Rate limiting

REST rate limiting is straightforward (limit by IP + endpoint). GraphQL is harder because one expensive query can behave like many REST requests.

Options:
1. **Request-level rate limit**: crude but simple. Rate limit by IP or user ID per minute.
2. **Complexity-based rate limit**: use query complexity scoring; bucket deductions against a per-user budget.
3. **Persisted queries**: only allow pre-registered queries (most restrictive, most secure, requires build-time coordination between client and server).

For public APIs: persisted queries. For internal APIs: complexity + request rate limiting.

## 7. Caching challenges

REST GET requests are cached at the HTTP layer for free. GraphQL uses POST — nothing caches by default.

Options:
1. **Apollo Cache-Control hints** + CDN: mark fields with `@cacheControl` directives; Apollo Server sets `Cache-Control` headers; CDN caches GET-style requests.
2. **Persisted queries over GET**: with APQ, cacheable queries can be sent as GET requests.
3. **Server-side caching**: cache resolver outputs in Redis with fine-grained TTLs.
4. **DataLoader** (within a single request): not a CDN cache, but prevents redundant fetches within one execution.

For public read-heavy APIs, caching is a significant design problem in GraphQL that REST handles for free.

## 8. Subscriptions at scale

WebSocket connections are stateful. They don't scale like stateless HTTP.

Production subscription requirements:
- **Shared PubSub backend** (Redis, etc.) so any server instance can push to any connected client.
- **Sticky sessions** or a dedicated subscription server if you have multiple API instances.
- **Auth at connection time AND on subscription start**.
- **Backpressure**: if the client is slow, don't buffer events indefinitely — drop or limit.
- **Reconnection handling** on the client: clients should reconnect with the last event ID and catch up.

If you only have a few hundred concurrent subscriptions, in-process PubSub is fine. Beyond that, Redis or NATS.

## 9. Schema evolution and versioning

GraphQL's official position: **no versioning**. Evolve the schema without breaking clients by:

1. **Adding fields is safe** — clients that don't request them are unaffected.
2. **Removing fields is breaking** — deprecate first with `@deprecated(reason: "...")`. Monitor which clients still use the field. Remove only when usage drops to zero.
3. **Changing types is breaking** — never change a field type. Add a new field with the new type.
4. **Renaming fields is breaking** — add the new name, deprecate the old name, monitor, then remove.

Tooling to enforce this: `graphql-inspector` can diff schemas and flag breaking changes in CI.

## 10. File uploads

The `multipart/form-data` upload convention (graphql-multipart-request-spec) is not part of the official spec and has security implications (CSRF via content-type sniffing).

Preferred approach for production: **pre-signed URLs**.

1. Client calls a mutation: `mutation { createUploadUrl(filename: "...) { url, fileId } }`.
2. Server returns a pre-signed S3 (or equivalent) URL.
3. Client uploads directly to S3 using the pre-signed URL.
4. Client calls another mutation to confirm: `mutation { confirmUpload(fileId: "...") { ... } }`.

This keeps large binary payloads off your GraphQL server entirely.
