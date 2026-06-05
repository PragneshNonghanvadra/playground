# GraphQL — Server and Client Libraries

## Server-side

### Apollo Server

The most widely used Node.js GraphQL server. Works standalone or as middleware (Express, Fastify, etc.).

```ts
import { ApolloServer } from '@apollo/server';
import { startStandaloneServer } from '@apollo/server/standalone';

const server = new ApolloServer({ typeDefs, resolvers });
const { url } = await startStandaloneServer(server, {
  context: async ({ req }) => ({ user: await getUser(req) }),
});
```

**Pros**: battle-tested, rich plugin ecosystem, built-in Apollo Studio integration, good caching primitives.

**Cons**: heavyweight for simple APIs; Apollo Studio requires sending operation telemetry to Apollo's cloud (disable with `apollo: { key: undefined }` if privacy matters).

**When to reach for it**: product-grade APIs where you want Apollo's ecosystem (federation, studio, etc.).

### GraphQL Yoga

Lighter-weight, modern, built on Fetch API (runs on Node, Deno, Bun, edge functions).

```ts
import { createSchema, createYoga } from 'graphql-yoga';
import { createServer } from 'node:http';

const yoga = createYoga({ schema: createSchema({ typeDefs, resolvers }) });
const server = createServer(yoga);
server.listen(4000);
```

**Pros**: small surface area, first-class edge/serverless support, plugin system.
**Cons**: smaller ecosystem than Apollo.
**When to reach for it**: edge functions, Bun/Deno, lightweight services, when Apollo's footprint is too much.

### Pothos (code-first, TypeScript)

Pothos is a code-first schema builder. You write TypeScript; it generates the SDL and resolver types.

```ts
import SchemaBuilder from '@pothos/core';

const builder = new SchemaBuilder<{ Context: { user: User | null } }>({});

builder.queryType({
  fields: (t) => ({
    post: t.field({
      type: Post,
      args: { id: t.arg.id({ required: true }) },
      resolve: (_, args, ctx) => db.post.findUnique({ where: { id: args.id } }),
    }),
  }),
});
```

**Pros**: one source of truth (TypeScript), rich plugin ecosystem, excellent type safety.
**Cons**: different way of thinking; steeper learning curve if you're used to SDL-first.
**When to reach for it**: TypeScript-first projects where SDL drift is painful.

### Choosing a server library

| | Apollo Server | GraphQL Yoga | Pothos |
|---|---|---|---|
| Footprint | Heavy | Light | Schema builder (works with either) |
| Code approach | Schema-first | Schema-first | Code-first |
| Type safety | Via codegen | Via codegen | Native TypeScript |
| Edge-ready | No | Yes | Schema-only |
| Ecosystem | Largest | Growing | Plugin-rich |

For learning: start with Apollo Server or Yoga + SDL. Move to Pothos when SDL/TS drift becomes painful.

---

## Client-side

### Apollo Client

Full-featured: normalized cache, queries, mutations, subscriptions, reactive state integration.

```ts
import { ApolloClient, InMemoryCache, gql } from '@apollo/client';

const client = new ApolloClient({
  uri: '/graphql',
  cache: new InMemoryCache(),
});

const { data } = await client.query({
  query: gql`query GetPost($id: ID!) { post(id: $id) { title } }`,
  variables: { id: '123' },
});
```

With React:
```tsx
function Post({ id }: { id: string }) {
  const { data, loading, error } = useQuery(GET_POST, { variables: { id } });
  if (loading) return <Spinner />;
  return <h1>{data.post.title}</h1>;
}
```

**Pros**: normalized cache means updates to one query automatically update all queries sharing the same object. Built-in optimistic UI. Full subscription support.

**Cons**: large bundle (~40KB). Cache can be hard to reason about for complex update patterns. Apollo-specific abstractions everywhere.

**When to use**: apps with lots of shared data across many queries where cache normalization pays off.

### urql

Smaller, more opinionated, extensible via exchanges (like middleware).

```ts
import { createClient, gql, cacheExchange, fetchExchange } from 'urql';

const client = createClient({
  url: '/graphql',
  exchanges: [cacheExchange, fetchExchange],
});
```

**Pros**: lighter (~17KB), more explicit cache model (document cache by default; opt into normalized cache), excellent SSR support.
**Cons**: less ecosystem, normalized cache requires an additional package.
**When to use**: when Apollo's footprint is overkill or you want more control over caching.

### React Query + plain fetch (no GraphQL client)

For teams that don't want GraphQL-specific client state, using React Query with a raw `fetch` works:

```ts
function usePost(id: string) {
  return useQuery({
    queryKey: ['post', id],
    queryFn: async () => {
      const res = await fetch('/graphql', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          query: `query GetPost($id: ID!) { post(id: $id) { title } }`,
          variables: { id },
        }),
      });
      const json = await res.json();
      if (json.errors) throw new Error(json.errors[0].message);
      return json.data.post;
    },
  });
}
```

**Pros**: no GraphQL client dependency, familiar React Query semantics, easy to replace later.
**Cons**: no normalized cache, no optimistic UI without manual work, more boilerplate.
**When to use**: lightweight needs, or when you're already heavily invested in React Query.

---

## Code generation

**The problem**: keeping TypeScript types in sync with the GraphQL schema is tedious and error-prone.

**The solution**: [@graphql-codegen](https://the-guild.dev/graphql/codegen) generates TypeScript types from your schema and operations.

```yaml
# codegen.yml
schema: './schema.graphql'
documents: './src/**/*.graphql'
generates:
  ./src/generated/graphql.ts:
    plugins:
      - typescript
      - typescript-operations
      - typescript-react-apollo   # or typescript-urql
```

After running `graphql-codegen`, you get typed hooks:

```ts
// Auto-generated
const { data } = useGetPostQuery({ variables: { id: '123' } });
// data.post.title is typed as string, not any
```

**This is table stakes for production GraphQL.** Without codegen, you're writing type assertions by hand and they'll drift.

---

## Persisted queries

Instead of sending the full query string on every request, clients send a hash. The server looks up the query by hash.

Benefits: smaller payloads, ability to whitelist known queries (reject arbitrary queries in production), easier rate limiting per operation.

Apollo calls this "Automatic Persisted Queries" (APQ). Most production GraphQL APIs should enable it.
