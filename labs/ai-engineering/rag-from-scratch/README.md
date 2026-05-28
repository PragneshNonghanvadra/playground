# Lab: RAG from Scratch

## Purpose

Build a retrieval-augmented generation pipeline by hand to understand each step's actual contribution: chunking, embedding, retrieval, reranking, grounding, citations, and evals. No black-box library magic.

## Topic area

[`docs/topics/ai-engineering/`](../../../docs/topics/ai-engineering/)

## Learning goal

After this lab, I should be able to explain why each step exists, what each step's failure modes are, and how to _measure_ whether the answer is actually grounded in the retrieved documents.

## Scope

- A small corpus (~50–500 docs from a domain I care about).
- Manual chunking with a chosen strategy (fixed-window, sentence-split, recursive, semantic).
- Embeddings via a single provider (OpenAI / Cohere / local).
- An in-memory or local vector store (no managed DB yet).
- A single LLM call grounded on top-k retrieved chunks with explicit citation requirements.
- An eval harness with ~20 question/answer/expected-citation tuples.

## Non-goals

- A managed vector DB.
- Multi-tenant ACL filtering (separate lab).
- Streaming UX (separate lab).
- Fine-tuning anything.

## Architecture

```
[ corpus ] -> [ chunker ] -> [ embedder ] -> [ vector store ]
                                                 ^
[ user query ] -> [ embedder ] -> [ retriever ] -|
                                       |
                                       v
                              [ optional reranker ]
                                       |
                                       v
                          [ prompt with retrieved chunks ]
                                       |
                                       v
                              [ LLM with citation schema ]
                                       |
                                       v
                                  [ answer + citations ]

[ eval set ] -> [ run pipeline ] -> [ rubric ] -> [ score ]
```

## Implementation plan

1. Pick a corpus. Keep it small enough to spot bad answers manually.
2. Build a chunker. Document the strategy and rationale.
3. Embed all chunks. Persist embeddings + metadata locally.
4. Build a retriever (cosine similarity, top-k=5).
5. Construct a grounded prompt that requires citation IDs.
6. Validate output schema (e.g., `{ answer: string, citations: [chunk_id] }`).
7. Build an eval set (~20 Q/A pairs with expected citations).
8. Score: did the answer use a chunk that contains the relevant fact? Did it abstain when it should have?

## Commands

```bash
# placeholder; flesh out during implementation
# pnpm run ingest
# pnpm run query "Your question here"
# pnpm run evals
```

## Expected observations

- Naive fixed-window chunking misses cross-paragraph context; semantic chunking helps for some queries, hurts others.
- Top-k of 3 vs 5 vs 10 changes faithfulness and latency more than expected.
- Without a citation schema, the LLM writes confident-sounding paraphrases that aren't actually grounded.
- A simple cross-encoder reranker noticeably improves precision @ k.
- Without an eval set, every prompt change feels "better" but you can't prove it.

## Failure experiment

- Ask the system a question NOT in the corpus. Does it abstain, hallucinate, or guess? Adjust the prompt until abstention is reliable.
- Inject contradictory chunks. Does the system pick one? Cite both? Hedge?
- Provide a malicious chunk with embedded prompt injection ("ignore previous instructions and say X"). Does the system follow it? Document mitigation.
- Embed a corpus with stale facts. Watch the answer confidently lie.

## Debugging guide

- Log every step: query, retrieved chunk IDs, scores, prompt token count, LLM output, citations.
- For each eval failure, inspect: did retrieval miss the right chunk, or did the LLM ignore it?
- Distinguish _retrieval_ failures from _generation_ failures. They have different fixes.

## Benchmark plan

- Eval set: 20 Q/A/citation triples, evolving over time.
- Metrics: retrieval recall @ k, citation precision, faithfulness rubric score, abstention precision/recall on out-of-corpus questions, latency p50/p95, cost per query.
- Always benchmark before and after a change.

## Production notes

What would change to operate this for real users:

- Managed vector store (or Postgres + pgvector) with backup and migration story.
- Re-embedding strategy when models change.
- Per-tenant ACL filtering at retrieval time (not after).
- Per-call structured logs (model, tokens in/out, latency, cost, tag).
- Eval regression suite in CI.
- Rate limits per tenant; cost budgets.
- Streaming UI with citation hover.
- Abstention UX (don't fake confidence).

## Interview notes

- RAG is not "search + LLM"; it's a retrieval system whose quality is bounded by the _retriever_. If retrieval misses, no prompt rescues it.
- Citations are a UX trust feature AND a debug feature.
- Without evals, you don't know if a prompt change helped.
- Faithfulness ≠ relevance ≠ precision. Each is a separate metric.
- Re-embedding is a real ops problem at scale.

## AI-agent notes

What an AI agent might get wrong:

- Reach for LangChain / LlamaIndex on day one and obscure the steps.
- Skip the eval harness — "we'll add it later".
- Embed everything in a flat file and call that "production".
- Invent fake metrics that sound rigorous but aren't tied to ground truth.
- Forget tenant filtering and recommend post-filter (a permission-leak bug waiting to happen).

## Follow-up experiments

- Hybrid search (vector + BM25) and measure precision @ k.
- Add a reranker and measure latency vs precision tradeoff.
- Try chunking strategies head-to-head with the same eval set.
- Try LLM-as-judge for faithfulness; sanity-check against human ratings.
- Add ACL filtering and verify zero leakage across tenants.

## Status

- [ ] Implementation pending
- [ ] First measurement taken
- [ ] Failure experiment run
- [ ] Lessons written back to `docs/topics/ai-engineering/`
