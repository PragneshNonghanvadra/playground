# GraphQL — Resolver Patterns

## What a resolver is

A resolver is a function that returns the value of one field. The runtime calls resolvers in document order, composing results.

```ts
const resolvers = {
  Query: {
    post: async (parent, args, context) => {
      return context.db.post.findUnique({ where: { id: args.id } });
    },
  },
  Post: {
    author: async (parent, args, context) => {
      // parent is the Post that was already resolved
      return context.db.user.findUnique({ where: { id: parent.authorId } });
    },
    comments: async (parent, args, context) => {
      return context.db.comment.findMany({ where: { postId: parent.id } });
    },
  },
};
```

The runtime merges resolver results into the response shape the client asked for.

## Default resolvers

If you don't define a resolver for a field, GraphQL uses the **default resolver**: `(parent) => parent[fieldName]`. This means for most scalar fields (id, title, body), you don't need to write anything — the default resolver reads from the parent object.

You only write resolvers when:
- The field requires a database lookup.
- The field name doesn't match the property name on the parent.
- The field requires business logic or transformation.

## The context object

Context is created once per request and passed to every resolver. It's the right place for:
- The authenticated user (`context.user`)
- Database clients (`context.db`)
- DataLoader instances (see below)
- Request headers or trace IDs

```ts
// Apollo Server example
const server = new ApolloServer({
  resolvers,
  typeDefs,
});

const handler = startStandaloneServer(server, {
  context: async ({ req }) => {
    const user = await authenticate(req.headers.authorization);
    return {
      user,
      db: prismaClient,
      loaders: createLoaders(prismaClient),
    };
  },
});
```

Never put per-request mutable state outside context. Context is the right isolation boundary.

## The N+1 problem

The most common performance failure in GraphQL. Here's how it happens:

```graphql
query {
  posts {        # 1 query → returns 10 posts
    author {     # 10 queries → one per post's authorId
      name
    }
  }
}
```

With the naive resolver above, fetching 10 posts fires 11 DB queries (1 + 10). With 100 posts: 101 queries. With pagination: every page triggers a burst.

This is a structural problem: resolvers are called per-field per-object, not batched.

## DataLoader — the standard fix

DataLoader batches individual lookups into a single bulk fetch per request tick:

```ts
import DataLoader from 'dataloader';

// Created per-request (inside context factory):
const userLoader = new DataLoader(async (userIds: readonly string[]) => {
  const users = await db.user.findMany({
    where: { id: { in: [...userIds] } },
  });
  // Must return results in the SAME ORDER as the input IDs
  return userIds.map(id => users.find(u => u.id === id) ?? null);
});

// Resolver uses it:
Post: {
  author: (parent, _, context) => context.loaders.user.load(parent.authorId),
}
```

What happens: all `author` resolver calls for posts in a single request are batched into one `userIds.map(id => ...)` call, which becomes one DB query. N+1 becomes 2.

**Key constraint**: DataLoader instances must be **created per request**, not shared. Sharing them across requests is a data-leakage bug.

## Auth in resolvers

Three places auth logic can live. Each has tradeoffs.

### 1. In each resolver (fine-grained, verbose)

```ts
Query: {
  adminDashboard: (_, __, context) => {
    if (!context.user?.isAdmin) throw new AuthorizationError('Admin required');
    return context.db.getAdminStats();
  },
}
```

Pro: explicit, co-located. Con: easy to forget on a new resolver.

### 2. Schema directives (declarative, centralized)

```graphql
type Query {
  adminDashboard: AdminStats @auth(requires: ADMIN)
  me: User @auth(requires: USER)
}
```

Enforced by a directive transformer. Pro: impossible to forget if schema is enforced. Con: directive transformers add complexity; schema-first coupling.

### 3. Middleware / resolver wrappers (e.g. graphql-shield)

```ts
const permissions = shield({
  Query: {
    adminDashboard: isAdmin,
    me: isAuthenticated,
  },
});
```

Pro: rules composable and testable in isolation. Con: another layer; error handling can surprise.

**Recommendation**: Use resolver-level checks for learning. Graduate to directive transformers or a shield library when the pattern is fully understood.

## Subscriptions

Subscriptions maintain a persistent connection (WebSocket) and push events to subscribed clients.

```ts
Subscription: {
  postPublished: {
    subscribe: (_, __, context) => context.pubsub.asyncIterator(['POST_PUBLISHED']),
    resolve: (payload) => payload.post,
  },
}

// Somewhere in a mutation resolver:
context.pubsub.publish('POST_PUBLISHED', { post: newPost });
```

Production concerns with subscriptions:
- WebSockets don't scale the same way as stateless HTTP. You need sticky sessions or a shared pubsub (Redis, etc.).
- Authentication must be validated at connection time AND on each subscription operation.
- Subscriptions can leak if connections are not cleaned up properly.
- Consider SSE over WebSocket for simple server-push with no client-to-server messages.

## Error handling in resolvers

Throw errors; let the runtime catch them:

```ts
import { GraphQLError } from 'graphql';

Query: {
  post: async (_, args, context) => {
    const post = await context.db.post.findUnique({ where: { id: args.id } });
    if (!post) {
      throw new GraphQLError('Post not found', {
        extensions: { code: 'NOT_FOUND', http: { status: 404 } },
      });
    }
    return post;
  },
}
```

The runtime catches thrown GraphQLErrors and places them in the `errors` array of the response.

Don't return `null` silently for "not found" on a non-nullable field — it cascades a null up the response tree and potentially nulls out an entire parent object.

## Resolver composition patterns

### Splitting resolvers by type (recommended)

Keep resolvers in one file per type, not one giant `resolvers.js`:

```
resolvers/
  query.ts
  mutation.ts
  post.ts
  user.ts
  comment.ts
```

Merge at the schema-building step. Every major library supports this.

### Code-first vs schema-first

**Schema-first**: write SDL → write resolvers that must match it. Good for teams that want to design the API first. Downside: SDL and TypeScript types are two sources of truth; code generators bridge the gap.

**Code-first**: write TypeScript (with a library like Pothos) → SDL is generated from the code. One source of truth. Better DX for TypeScript teams. Downside: SDL is not the designed artifact — it's the output.

Pick one approach per project and don't mix.
