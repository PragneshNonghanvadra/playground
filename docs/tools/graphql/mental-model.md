# GraphQL — Mental Model

## The core mental model

> GraphQL is a **typed query language over a graph of your data**.
> The server publishes *what exists*. The client asks for *what it needs*.

Three pieces:

1. **Schema** — a type system that describes every object, field, and relationship available. Think of it as the API contract. It lives on the server and is the single source of truth.

2. **Operations** — a client-written document that declares what it wants from that schema (query / mutation / subscription). Think of it as a SQL `SELECT` — but for your API.

3. **Resolvers** — functions on the server that know how to fetch the value of each field. The runtime calls them in order, composing the result. Think of them as tiny focused data-fetchers, one per field.

```
Client writes:
  query {
    post(id: "123") {
      title
      author {
        name
      }
    }
  }

Server schema says this is valid (Post has title, author; Author has name).
Server resolvers fetch: Post → Author.
Server returns exactly: { post: { title: "...", author: { name: "..." } } }
```

## What "graph" actually means

The "graph" in GraphQL is not a graph database. It refers to the fact that your data model is naturally a **graph of connected objects** — posts have authors; authors have posts; posts have comments; comments have authors.

REST flattens this into a hierarchy of URL endpoints. GraphQL lets the client traverse the graph however it needs, in one request.

## What the runtime does

When a query arrives:

1. **Parse** — turn the query string into an AST.
2. **Validate** — check the AST against the schema. Reject anything that doesn't conform before execution begins.
3. **Execute** — walk the AST, calling the resolver for each field, composing results.

The validation step is significant: invalid queries are rejected *before any data is fetched*. This is why GraphQL tooling (Apollo Sandbox, IntelliJ plugins, etc.) can give you compile-time-like feedback on your queries.

## The resolver as the fundamental unit

A resolver is just a function:

```ts
type Resolver<Parent, Args, Context, Return> = (
  parent: Parent,     // the resolved value of the parent field
  args: Args,         // arguments from the query (e.g., id: "123")
  context: Context,   // shared context per request (auth user, dataloaders, db)
  info: GraphQLResolveInfo  // AST info about this field (rarely needed)
) => Return | Promise<Return>
```

The runtime handles the orchestration. You write isolated field functions; GraphQL composes them.

## What makes this different from REST

In REST, the **server** defines the response shape. In GraphQL, the **client** defines the response shape from a menu the server provides.

REST:
```
GET /posts/123/with-author-and-comments
→ { id, title, body, authorId, author: { id, name, bio }, comments: [...] }
(whether you wanted bio and all comments or not)
```

GraphQL:
```
query { post(id: "123") { title, author { name } } }
→ { post: { title: "...", author: { name: "..." } } }
(exactly what was asked for)
```

## The mental shift to internalize

Stop thinking in **endpoints**. Start thinking in **types and fields**.

Your job on the server is no longer "design a URL that returns the right shape". It's "define a type system that accurately describes your domain, and write a resolver for each field that knows how to fetch its value."

The composition is GraphQL's job. Your job is the schema and the individual resolvers.
