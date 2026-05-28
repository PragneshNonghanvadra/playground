# Architecture

Architecture artifacts for Playground.

## Layout

- [`adr/`](adr/) — Architecture Decision Records. Numbered, immutable once accepted. Use [`templates/adr-template.md`](../../templates/adr-template.md).
- [`rfcs/`](rfcs/) — Larger design proposals before they become decisions. Use [`templates/rfc-template.md`](../../templates/rfc-template.md).
- [`diagrams/`](diagrams/) — Saved diagrams (SVG, PNG, Mermaid).
- [`c4/`](c4/) — C4-model views (context, container, component, code) for major systems.
- [`tradeoffs/`](tradeoffs/) — Tradeoff analyses using [`templates/tradeoff-template.md`](../../templates/tradeoff-template.md).

## When to write what

| Artifact     | Use it when                                                                      |
| ------------ | -------------------------------------------------------------------------------- |
| ADR          | A decision was made. Even small ones, if non-obvious.                            |
| RFC          | A decision is being _considered_. Especially when alternatives need exploration. |
| Diagram      | A picture would save 1000 words of prose.                                        |
| C4 view      | A system has enough complexity to need layered views.                            |
| Tradeoff doc | Comparing 3+ options where the choice depends on context.                        |

## Index of decisions

> When ADRs accumulate, list them here in order with one-line summaries.

- [ADR-0001](adr/0001-use-playground-as-learning-lab.md) — Use Playground as a learning lab, not a product.

## Index of RFCs

> When RFCs accumulate, list them here.

_None yet._
