# Documentation Standards

These standards apply to every doc in this repo. They exist so future-me (and future AI agents) can navigate consistently.

## File naming

- Lowercase, kebab-case: `mental-model.md`, `failure-modes.md`.
- ADRs are numbered: `0001-<slug>.md`.
- One topic per file. If a file gets long, split it and link.

## Front matter

Front matter is **optional**. If used, it should declare:

```yaml
---
status: draft | living | stable | archived
last_updated: YYYY-MM-DD
confidence: shallow | working | deep | teaching
related: [topics/backend-systems, topics/observability]
---
```

Front matter is encouraged for topic notes but not required.

## Headings

- One `#` per file (the title).
- Use `##` and `###` liberally.
- Headings should be navigable from a table of contents in your head.

## Voice

- First-person is fine. This is a personal lab.
- Plain language. No jargon you can't define.
- Concrete > abstract. Numbers > "it depends" (when honest).

## Linking

- Cross-link aggressively between topic notes.
- Use relative paths.
- Update [`docs/concept-map/connections.md`](../concept-map/connections.md) when adding new cross-topic links.

## Citing sources

- Quote authors faithfully.
- Prefer primary sources (papers, docs) over summaries.
- Note publication date if recency matters.

## Confidence labels

When making a claim, optionally tag confidence:

- `[verified]` — I've confirmed this from primary sources or built it.
- `[working knowledge]` — I'm confident enough to use it; not bulletproof.
- `[hearsay]` — I read this somewhere; haven't verified.
- `[guess]` — speculation worth marking.

## Diagrams

- ASCII diagrams are first-class.
- For richer diagrams, prefer Mermaid (renders in many viewers) or commit an SVG to `docs/architecture/diagrams/`.
- Always include a text description. A diagram alone is not a doc.

## Templates

If a [template](../../templates/) exists, use it. Don't reinvent structure for each note.

## Length

- A note that is too long to re-read is a note that won't be re-read. Split.
- A note that is too short to teach is a stub. Mark it as such with `> Stub. Returns soon.`

## Updating notes

- Old notes are precious data. **Don't overwrite — annotate.**
- When my thinking changes, add a dated note: `> 2026-04-12 update: I now disagree with section 7 because…`.
- When a note is wrong, mark it but keep it: `> SUPERSEDED by [link]. Kept for reference.`

## TODOs and uncertainty

- Capture unknowns as questions, not vague comments.
- Add durable open questions to [`docs/concept-map/open-questions.md`](../concept-map/open-questions.md).
- Mark inline uncertainty with `> Uncertain: …` blocks rather than burying it in prose.
