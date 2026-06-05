# GraphQL — Core Concepts

## Schema Definition Language (SDL)

The schema is the contract between client and server. Written in SDL:

```graphql
type Post {
  id: ID!
  title: String!
  body: String
  published: Boolean!
  author: User!
  comments: [Comment!]!
  createdAt: String!
}

type User {
  id: ID!
  name: String!
  email: String!
  posts: [Post!]!
}

type Comment {
  id: ID!
  body: String!
  author: User!
}
```

**`!` means non-null.** `String!` = never null. `String` = may be null. `[Comment!]!` = the list is never null, and no item in the list is null.

## The three root types

Every schema has these as entry points:

```graphql
type Query {
  post(id: ID!): Post          # read; safe to cache
  posts(filter: PostFilter): [Post!]!
  me: User
}

type Mutation {
  createPost(input: CreatePostInput!): Post!   # write; NOT safe to cache
  updatePost(id: ID!, input: UpdatePostInput!): Post!
  deletePost(id: ID!): Boolean!
}

type Subscription {
  postPublished: Post!   # real-time; server pushes to client
  commentAdded(postId: ID!): Comment!
}
```

- `Query` — read operations. Can be parallelized by the runtime.
- `Mutation` — write operations. Run sequentially if multiple mutations in one request.
- `Subscription` — server-push over a persistent connection (WebSocket or SSE).

## Scalar types

Built-in: `Int`, `Float`, `String`, `Boolean`, `ID`.

`ID` is a special scalar — serialized as a string, means "this is an opaque identifier". Use it for primary keys.

Custom scalars are possible (e.g., `DateTime`, `URL`, `JSON`). They require a serialize/parse/literal implementation.

## Input types

Mutations take **input types**, not regular types. Input types can only contain scalars and other input types — no circular references, no resolvers.

```graphql
input CreatePostInput {
  title: String!
  body: String
  published: Boolean! = false
}
```

The distinction between a `type` (output) and an `input type` is strict. You can't reuse the same type for both.

## Queries — three forms

```graphql
# Shorthand (anonymous, for quick exploration)
{ post(id: "1") { title } }

# Named (recommended for production)
query GetPost($id: ID!) {
  post(id: $id) {
    title
    author { name }
  }
}

# With multiple fields
query Dashboard {
  me { name }
  posts { title, published }
}
```

Always use named queries in production. Anonymous queries are harder to trace in logs and can't be persisted.

## Variables

Variables separate query structure from runtime values. Never string-interpolate values into a query — that's the GraphQL equivalent of SQL injection.

```json
// Variables document (sent alongside the query)
{ "id": "123" }
```

```graphql
query GetPost($id: ID!) {
  post(id: $id) { title }
}
```

## Fragments

Reusable selections of fields on a type. The client-side equivalent of a function:

```graphql
fragment PostCard on Post {
  id
  title
  author { name }
  createdAt
}

query Feed {
  posts {
    ...PostCard      # spread the fragment here
  }
}
```

Why they matter: fragments are the unit of **component co-location**. In a React app, each component declares the fragment it needs; the top-level query composes all fragments. This means components are self-contained about their data requirements.

## Aliases

Request the same field twice with different arguments:

```graphql
query {
  published: posts(filter: { published: true }) { title }
  drafts: posts(filter: { published: false }) { title }
}
```

## Directives

Built-in:
- `@include(if: Boolean)` — conditionally include a field.
- `@skip(if: Boolean)` — conditionally skip a field.

Custom directives can be added server-side for auth (`@auth(requires: ADMIN)`), rate limiting, deprecation, formatting, etc.

## Introspection

GraphQL schemas are **self-describing**. You can query the schema itself:

```graphql
query {
  __schema {
    types { name kind }
  }
  __type(name: "Post") {
    fields { name type { name } }
  }
}
```

This powers GraphiQL, Apollo Sandbox, code generators, and type-safe client libraries. **Disable or restrict introspection in production** — it reveals your full schema to anyone who can reach the endpoint.

## Errors

GraphQL always returns HTTP 200 (even for errors — this is controversial and confusing).

The response envelope:

```json
{
  "data": {
    "post": null
  },
  "errors": [
    {
      "message": "Post not found",
      "locations": [{ "line": 2, "column": 3 }],
      "path": ["post"],
      "extensions": { "code": "NOT_FOUND" }
    }
  ]
}
```

Key rule: **partial success is a thing**. `data` can be partially populated with some fields and `errors` can simultaneously have entries for the failed fields. This is GraphQL's explicit design; most systems that replace GraphQL with REST hide this nuance.

Use `extensions.code` for machine-readable error codes. Libraries like Apollo Server map exceptions to error shapes here.

## Pagination conventions

No pagination is built into the spec. Two common conventions:

**Offset-based** (simpler):
```graphql
posts(limit: 10, offset: 20): [Post!]!
```

**Cursor-based / Relay-style** (correct for large, live data):
```graphql
posts(first: 10, after: "cursor"): PostConnection!

type PostConnection {
  edges: [PostEdge!]!
  pageInfo: PageInfo!
}

type PostEdge {
  node: Post!
  cursor: String!
}

type PageInfo {
  hasNextPage: Boolean!
  endCursor: String
}
```

Prefer Relay-style cursor pagination for anything that scrolls infinitely or has data that changes while paginating.
