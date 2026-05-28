# AI Agent Review: <Change Title>

## Date

YYYY-MM-DD

## Agent / model

Which agent, which model, which prompt or skill.

## Task given to agent

What was the agent asked to do? Quote the prompt verbatim.

## What the agent produced

A summary of the diff or output. Link to commits / PR if applicable.

## What was correct

Specifics: which decisions, code, or notes were good.

## What was wrong

Specifics: hallucinations, wrong APIs, wrong tradeoffs, missing failure modes, premature abstractions, dead code, made-up dependencies.

## What was almost right

Subtler issues: technically working but conceptually off, or solving the wrong problem.

## What was missing

Things a senior engineer would have included that the agent didn't (failure modes, observability, auth, idempotency, ADR, etc.).

## Architectural critique

Did the agent reach for heavyweight infrastructure? Did it propose microservices, queues, or AI abstractions without need? Did it ignore the simpler path?

## Decisions I overrode

Where I disagreed and why. Cite topic notes.

## Decisions I accepted

Where the agent was right or close enough. Cite topic notes.

## Lessons for future prompts

How to write better instructions to avoid this class of mistake next time.

## Move to one of

- `ai-agent/accepted-suggestions/` — strong, reusable patterns
- `ai-agent/rejected-suggestions/` — patterns to actively reject
- `ai-agent/hallucinations/` — fabrications worth remembering
- `ai-agent/architecture-critiques/` — non-trivial design pushback
