# Labs: Tools

Tiny experiments for specific tools and frameworks. Each lab tests one concrete behavior — not "learn GraphQL" but "observe N+1 in action and measure DataLoader's fix".

## Conventions

- One subfolder per tool: `labs/tools/<tool-name>/`.
- Each lab's `README.md` follows [`templates/lab-template.md`](../../templates/lab-template.md).
- Every non-trivial lab has a measurement and a failure experiment.
- Lessons feed back into [`docs/tools/<tool-name>/`](../../docs/tools/).

## Index

| Lab | Tool | Teaches |
|---|---|---|
| [graphql-fundamentals/](graphql-fundamentals/) | GraphQL | Schema, resolvers, N+1/DataLoader, auth gap, depth limits |
