# AI Engineering

## Why this topic matters

AI engineering is the discipline of **shipping reliable products on top of unreliable models**. The interesting work is not "use an LLM"; it's the surrounding system: structured outputs, evaluations, prompt design, retrieval, tool calling, cost control, safety, and observability.

Most AI products fail not because the model is bad, but because the team:

- has no eval (so they can't tell when it gets worse)
- has no logging (so they can't debug)
- has no schema (so they can't trust the output)
- has no cost model (so they can't price)
- has no safety boundary (so the system does the wrong thing politely)

For someone moving from frontend toward AI-native full-stack, this is **the** highest-leverage area, but only if you treat it with the same rigor as backend systems — not as magic.

## Subtopics inside this area

- **LLM fundamentals** — tokens, context windows, sampling (temperature, top-p), determinism, cost shape.
- **Prompt design** — system vs user; instructions vs examples; few-shot; structured prompts; evaluating prompts the same way you evaluate code.
- **Structured outputs** — JSON mode, schema-constrained generation, retry-on-invalid, partial parse.
- **Embeddings** — what they actually represent; choosing dimensions; refresh / re-embedding strategy.
- **RAG** — chunking, retrieval, reranking, citation, grounding; the difference between "retrieves stuff" and "answers correctly".
- **Vector search** — ANN indexes (HNSW, IVF), hybrid search (vector + keyword + filters), permission filtering.
- **Reranking** — cross-encoder rerankers; latency vs quality tradeoffs.
- **Tool calling** — function schemas, validation, allowlists, retries, sandboxing.
- **Agents & workflows** — durable execution, step limits, kill switches, human-in-the-loop.
- **Evals** — golden datasets, rubrics, LLM-as-judge, regression suites; eval-driven development.
- **Cost & latency** — per-call budgets, model routing, caching responses, streaming UX.
- **Safety** — prompt injection, indirect injection, output validation, exfiltration, abuse vectors.
- **Determinism & reproducibility** — seeds, snapshots, model versioning.
- **Fine-tuning vs prompting vs RAG** — when each is the right tool.

## How this connects to other topics

| Connects to             | How                                                                                 |
| ----------------------- | ----------------------------------------------------------------------------------- |
| **Backend systems**     | Tool calling = backend RPC with a schema. Idempotency, retries, timeouts all apply. |
| **Data systems**        | Embeddings, vector indexes, hybrid retrieval, ACL-aware search.                     |
| **Distributed systems** | Agent orchestration is durable workflow. Retries, compensation, tail latency.       |
| **Observability**       | Per-call logs (model, tokens, latency, cost) are non-negotiable. AI traces.         |
| **Security**            | Prompt injection, output validation, sandboxing, agent identity.                    |
| **Product engineering** | Trust UX (citations, abstention, "why this answer"); feedback loops feeding evals.  |
| **AI-native UX**        | Streaming, partial states, recovery, latency-aware design.                          |

## Suggested first experiments

1. **`labs/ai-engineering/rag-from-scratch`** — Tiny corpus, manual chunking, OpenAI/Cohere embeddings, in-memory vector store, ANN retrieval, an LLM call grounded on retrieved docs. Measure: precision @ k, faithfulness (rubric), latency, cost per query.
2. **Structured-output reliability** — Ask an LLM for JSON. Measure validation failure rate. Compare: (a) prose prompt, (b) JSON-mode, (c) schema-constrained, (d) retry-on-invalid.
3. **Eval harness for a tiny task** — Build 20 examples, a rubric, and an LLM-as-judge. Run two prompts. Run again with a different model. Compare.
4. **Tool calling with a guardrail** — One safe tool, one dangerous tool (`delete_user`). Allowlist + human approval gate. Try to jailbreak it.
5. **Cost log + dashboard** — Wrap every LLM call with a logger that records tokens in/out, latency, model, and tag. Plot cost per task and cost per user.

## Interview relevance

Increasingly common for full-stack and AI-product roles:

- "How would you build a Q&A over our docs?" (RAG: chunking, retrieval, grounding, citations, evals)
- "How do you know it's working?" (evals — and the answer is _not_ "we look at outputs")
- "How do you control LLM costs?" (caching, routing, batching, prompt design, smaller models)
- "How do you keep an agent from doing something dangerous?" (tool allowlists, validation, sandboxing, human approval)
- "What is prompt injection?" (with at least one indirect-injection example)
- "How do you fine-tune?" (and the senior signal: usually you don't first — RAG and prompting are cheaper)

Strong answers always include **eval plan**, **logging plan**, **failure modes** (hallucination, injection, drift), and a **cost model**.

## Topic notes in this folder

- `mental-model.md`, `why-it-matters.md`, `must-know.md`, `good-to-know.md`, `beginner-mistakes.md`, `senior-engineer-thinking.md`, `ai-native-perspective.md`, `production-concerns.md`, `failure-modes.md`, `scaling-considerations.md`, `tradeoffs.md`, `interview-notes.md`, `resources.md`, `experiments.md`, `connected-concepts.md`, `open-questions.md`.
