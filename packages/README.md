# packages/

Shared packages that have **graduated from labs**. A lab graduates when:

1. It has been used in at least one other lab or service.
2. It has a stable interface.
3. It has an ADR explaining the design and reversal triggers.
4. It has tests and basic docs.

## Subfolders

- [`ui/`](ui/) — design-system primitives
- [`config/`](config/) — shared configuration loading
- [`contracts/`](contracts/) — API and event schemas (single source of truth)
- [`db/`](db/) — database client + migration tooling
- [`auth/`](auth/) — authentication and authorization primitives
- [`observability/`](observability/) — logger, metrics, tracer wrappers
- [`evals/`](evals/) — eval harness for AI features
- [`agent-tools/`](agent-tools/) — tool schemas + execution for LLM tool calling
