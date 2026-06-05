# GraphQL — Resources

## Primary sources (start here)

- [GraphQL Spec](https://spec.graphql.org/) — the canonical specification. Not a tutorial, but worth skimming sections 2 (language) and 6 (execution) once you have the basics.
- [graphql.org/learn](https://graphql.org/learn) — official intro. Good for the fundamentals; stop before "Best Practices" starts suggesting things that don't hold in large systems.
- [How to GraphQL](https://www.howtographql.com/) — free, full-stack tutorial for multiple languages. Good hands-on start.

## The one book worth reading

- **Production Ready GraphQL** by Marc-André Giroux — the best single resource for non-trivial systems. Covers schema design, performance, auth, versioning, and real-world tradeoffs. Not beginner-level; read after the fundamentals.

## Server libraries

- [Apollo Server docs](https://www.apollographql.com/docs/apollo-server/) — thorough. Note: some docs push Apollo-specific cloud features.
- [GraphQL Yoga docs](https://the-guild.dev/graphql/yoga-server/docs) — clean, modern. Good for edge/serverless.
- [Pothos docs](https://pothos-graphql.dev/) — code-first. Worth reading if you're TypeScript-first.

## Client libraries

- [Apollo Client docs](https://www.apollographql.com/docs/react/) — comprehensive but large.
- [urql docs](https://commerce.nearform.com/open-source/urql/) — worth reading the "concepts" section to understand their exchange model vs Apollo's cache model.

## DataLoader

- [DataLoader GitHub](https://github.com/graphql/dataloader) — read the README end-to-end. The batching algorithm is explained clearly and you need to understand it to use DataLoader correctly.

## Codegen

- [graphql-code-generator](https://the-guild.dev/graphql/codegen) — the standard. Read the "getting started" and the React/Apollo plugin section.

## Schema evolution and breaking changes

- [graphql-inspector](https://github.com/nicholasgasior/graphql-inspector) — CI schema diffing. Set this up early.
- [GraphQL's versioning post](https://graphql.org/learn/best-practices/#versioning) — the official "no versioning" argument.

## Performance

- [Understanding the N+1 Problem - Apollo Blog](https://www.apollographql.com/blog/backend/graphql-explained/graphql-performance-best-practices/) — decent starting point.
- [Benjie Gillam's GraphQL performance talks](https://www.youtube.com/watch?v=...) — search for his GraphQL talks; consistently high quality.

## Security

- [OWASP GraphQL Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/GraphQL_Cheat_Sheet.html) — worth reading once before production.
- [graphql-armor](https://github.com/nicolo-ribaudo/graphql-armor) — security middleware plugin for Yoga/Apollo (depth limit, complexity, field suggestions, etc.).

## Status

- [ ] graphql.org/learn — read
- [ ] DataLoader README — read
- [ ] Production Ready GraphQL — to read
- [ ] OWASP GraphQL Cheat Sheet — to read before production
