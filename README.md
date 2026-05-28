# Playground

Playground is a long-term engineering learning lab.

It exists to help me develop deep engineering judgment across:

- core engineering foundations
- frontend architecture
- backend systems
- distributed systems
- AI engineering
- data systems
- infrastructure
- observability
- security
- performance
- product engineering
- system design
- AI-native UX
- engineering leadership
- founder mindset

This repo is **not** a tutorial collection and **not** a single product.

It is a place to:

- study concepts
- write mental models
- build experiments
- document tradeoffs
- record failure modes
- create architecture notes
- run benchmarks
- practice system design
- use AI coding agents critically
- evolve experiments into real products

## Core philosophy

Treat this repository as a **modular learning operating system**, not a product.

Every topic moves through this lifecycle:

```
Idea / Curiosity
  -> Topic notes
  -> Mental model
  -> Real-world examples
  -> Tradeoffs
  -> Failure modes
  -> Tiny experiment
  -> Playground module
  -> Production notes
  -> Interview notes
  -> Connected concepts
  -> Eventually reused in a real project
```

The repo supports both **learning-first exploration** and **implementation-first experiments**.

Sometimes I will only study and document a concept. Sometimes I will build a small proof of concept. Sometimes I will convert the concept into a reusable module. Sometimes I will combine multiple modules into a real product. The structure supports all of these paths.

## Core workflow

1. Pick a topic.
2. Write why it matters.
3. Explain the mental model.
4. Study real-world usage.
5. Document mistakes and tradeoffs.
6. Build a tiny lab.
7. Break it intentionally.
8. Add production notes.
9. Add interview notes.
10. Connect it to other topics.

## Repository map

| Folder                   | Purpose                                                                                                                 |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------- |
| `docs/`                  | Markdown-first knowledge base: topic notes, architecture, interviews, resources, glossary, concept map.                 |
| `docs/operating-system/` | The "how I learn and decide" rules: principles, lifecycle, frameworks, checklists.                                      |
| `docs/topics/`           | One folder per engineering area, with structured notes (mental model, tradeoffs, failure modes, interview notes, etc.). |
| `docs/architecture/`     | ADRs, RFCs, diagrams, C4 models, tradeoff analyses.                                                                     |
| `docs/interviews/`       | Interview-focused notes per area.                                                                                       |
| `docs/resources/`        | Books, courses, blogs, papers, videos, repos.                                                                           |
| `docs/glossary/`         | Shared vocabulary.                                                                                                      |
| `docs/concept-map/`      | Cross-topic connections, open questions, learning backlog.                                                              |
| `labs/`                  | Tiny, focused experiments grouped by topic.                                                                             |
| `apps/`                  | Placeholder UIs (web, admin, docs-site) for future product slices.                                                      |
| `services/`              | Placeholder backend services (api, worker, ai-gateway, realtime, ingestion).                                            |
| `packages/`              | Placeholder shared packages (ui, config, contracts, db, auth, observability, evals, agent-tools).                       |
| `infra/`                 | Placeholder infra-as-code (docker, terraform, kubernetes, local-dev).                                                   |
| `evals/`                 | Datasets, rubrics, regression suites for AI work.                                                                       |
| `benchmarks/`            | Performance and cost benchmarks across stacks.                                                                          |
| `scripts/`               | Placeholder generators (create-topic, create-lab, create-adr, create-rfc, create-interview-note).                       |
| `templates/`             | Source-of-truth templates for topics, labs, ADRs, RFCs, interviews, etc.                                                |
| `ai-agent/`              | Instructions, prompts, and review logs for AI coding agents working in this repo.                                       |

## How to add a topic

1. Decide the topic's category (`docs/topics/<category>/`).
2. Copy `templates/topic-template.md` into a new file or edit the topic's existing notes.
3. Fill in stages incrementally — see [`docs/operating-system/topic-lifecycle.md`](docs/operating-system/topic-lifecycle.md).
4. Update [`docs/concept-map/connections.md`](docs/concept-map/connections.md) with new cross-topic links.
5. If interview-relevant, add an entry under `docs/interviews/<area>/`.

## How to add a lab

1. Find the matching topic folder under `labs/<category>/`.
2. Create a new subfolder for your lab.
3. Use [`templates/lab-template.md`](templates/lab-template.md) as the lab `README.md`.
4. Keep it small — a lab should fit in your head.
5. Always include at least one failure experiment and at least one measurement.

## How to use AI agents

AI coding agents are **collaborators, not decision makers**.

Before letting an AI agent touch this repo, read:

- [`.ai-context.md`](.ai-context.md)
- [`ai-agent/instructions.md`](ai-agent/instructions.md)
- [`ai-agent/coding-agent-rules.md`](ai-agent/coding-agent-rules.md)

Critique AI-generated architectures. Log accepted, rejected, and hallucinated suggestions under `ai-agent/`. The goal is not to ship more AI code; it is to develop better engineering judgment about AI's outputs.

## Preferred rhythm

**Weekly**: Pick 1–2 topics. Read high-quality resources. Fill topic notes. Add beginner mistakes and senior thinking. Build 1 tiny lab if useful. Add a failure experiment. Add interview explanation. Connect it to other topics.

**Monthly**: Convert useful experiments into reusable modules. Write ADRs for important decisions. Update the concept map. Review AI-agent mistakes. Summarize what changed in my thinking.

**Quarterly**: Combine selected modules into a real product slice. Review architecture evolution. Benchmark and evaluate. Write production-readiness notes.

## Current focus

> _Update this section as you start. Keep it short — at most 1–2 topics and 1 lab in flight._

- **Topics in flight**: _TBD_
- **Lab in flight**: _TBD_
- **Open questions worth resolving this week**: _TBD_

## Quality bar

- **Clarity over cleverness.** Write so future-me can pick it up cold.
- **Boring tech by default.** Reach for novelty only when the lab is _about_ novelty.
- **Document tradeoffs and failure modes.** No decision is complete without them.
- **Measure something.** Latency, correctness, cost, memory, UX — pick at least one.
- **Connect concepts.** Every topic should link to at least one related topic.
- **Track uncertainty.** Open questions are first-class artifacts, not loose ends.
