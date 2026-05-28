# services/

Placeholder for backend services. Intentionally empty until justified by accumulated lab work.

## Subfolders

- [`api/`](api/) — main HTTP API
- [`worker/`](worker/) — background job worker
- [`ai-gateway/`](ai-gateway/) — LLM call routing, logging, evals
- [`realtime/`](realtime/) — websockets / live sync
- [`ingestion/`](ingestion/) — data ingestion pipelines

## Quality gate

Same as `apps/`. No service starts here without an ADR.
