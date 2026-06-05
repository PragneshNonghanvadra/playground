# Tools

Deep-dive notes on specific tools and frameworks, studied independently.

## Philosophy

Tools are explored here on their own terms — **without blocking on prerequisite topic coverage**.
When a related topic (backend-systems, distributed-systems, etc.) eventually gets studied,
the connection gets made explicitly in `connected-concepts.md` inside each tool folder.

This is intentional: don't block yourself on "I should understand REST deeply before GraphQL".
If you already know the concept roughly, explore the tool now and document the connection later.

## Structure

Each tool gets its own subfolder:

```
docs/tools/<tool-name>/
  README.md               - overview, when to use, positioning
  mental-model.md         - the core mental model specific to this tool
  core-concepts.md        - the irreducible vocabulary and building blocks
  resolver-patterns.md    - (or equivalent: tool-specific patterns)
  production-concerns.md  - what breaks in production
  failure-modes.md        - concrete failure scenarios
  tradeoffs.md            - vs alternatives (REST, gRPC, etc.)
  interview-notes.md      - 30s / 2m / deep-dive
  resources.md            - curated references
  experiments.md          - lab ideas
  connected-concepts.md   - links to topic notes (fill in later)
```

Not every tool needs all files. Start with what matters; add later.

## Index

| Tool | Category | Status |
|---|---|---|
| [rest/](rest/) | API | Reference (already known) |
| [graphql/](graphql/) | API | In progress |

## Labs

Experiments live under [`labs/tools/`](../../labs/tools/).
