# docs/

The knowledge base. Markdown-first. This is the most important folder in the repo.

## Layout

- [`operating-system/`](operating-system/) — how I learn and decide. Principles, lifecycle, frameworks, checklists.
- [`topics/`](topics/) — one folder per engineering area, with structured notes (mental model, tradeoffs, failure modes, interview notes, etc.). Index in [`topics/README.md`](topics/README.md).
- [`tools/`](tools/) — deep-dives on specific tools and frameworks, studied independently. No prerequisite topic coverage required; connect to topics later. Index in [`tools/README.md`](tools/README.md).
- [`architecture/`](architecture/) — ADRs, RFCs, diagrams, C4 models, tradeoff analyses.
- [`interviews/`](interviews/) — interview-focused notes per area.
- [`resources/`](resources/) — books, courses, blogs, papers, videos, repos.
- [`glossary/`](glossary/) — shared vocabulary across all notes.
- [`concept-map/`](concept-map/) — cross-topic connections, open questions, learning backlog.

## Reading order for a new visitor (or a future me, in 6 months)

1. [`README.md`](../README.md) at the repo root.
2. [`operating-system/learning-principles.md`](operating-system/learning-principles.md).
3. [`operating-system/topic-lifecycle.md`](operating-system/topic-lifecycle.md).
4. [`concept-map/connections.md`](concept-map/connections.md) — get a sense of what connects to what.
5. Pick a topic in [`topics/`](topics/) and read its `README.md`.

## Contributing (to my own future self)

- Use the templates under [`templates/`](../templates/) where they fit.
- Run the relevant [review checklist](operating-system/review-checklists.md) before marking a note "stable".
- Update [`concept-map/connections.md`](concept-map/connections.md) when adding cross-topic links.
- Be honest about uncertainty. Mark stubs as stubs.
