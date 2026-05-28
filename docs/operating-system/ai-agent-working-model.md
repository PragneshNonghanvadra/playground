# AI Agent Working Model

How I use AI coding agents in this repo. The goal is **leverage with judgment**, not autopilot.

## Default mental model

> AI agents are extremely fast junior engineers with infinite stamina, no organizational memory, and overconfident pattern-matching.

Treat them accordingly. They are excellent at:

- Generating boilerplate
- Translating ideas across stacks
- Summarizing unfamiliar code
- Drafting documentation
- Producing first drafts of tests, types, schemas
- Exploring API surfaces

They are weak at:

- Choosing architectures under real constraints
- Naming source of truth correctly
- Anticipating partial failure
- Distinguishing necessary complexity from accidental complexity
- Recognizing when _not_ to use a tool
- Honesty about uncertainty

## Three modes of using agents

### 1. Drafter mode

I have a clear plan. The agent generates the first draft. I edit aggressively. Used for boilerplate, scaffolding, repetitive notes.

### 2. Explorer mode

I'm exploring an unfamiliar area. The agent surfaces options, terminology, and references. I treat all output as _hypotheses_ and verify against primary sources.

### 3. Critic mode

I draft something. The agent critiques. Useful for surfacing missed failure modes, alternatives, or edge cases. Still verify.

I never let an agent operate in **architect mode** — making irreversible architectural decisions — without explicit review.

## Required habits

1. **Read [`ai-agent/coding-agent-rules.md`](../../ai-agent/coding-agent-rules.md) before delegating anything non-trivial.**
2. **Before delegating**: state the topic, the constraints, what's in scope, what's out.
3. **After delegating**: run the [AI Agent Review Checklist](review-checklists.md#ai-agent-review-checklist) and log notable outcomes under `ai-agent/`.
4. **Never accept an architectural choice without alternatives and reversal triggers.**
5. **Never accept AI-touching code without an eval plan and a logging plan.**

## Logging

For any non-trivial agent task, fill out [`templates/ai-agent-review-template.md`](../../templates/ai-agent-review-template.md) and file it under one of:

- `ai-agent/accepted-suggestions/`
- `ai-agent/rejected-suggestions/`
- `ai-agent/hallucinations/`
- `ai-agent/architecture-critiques/`

This isn't bureaucracy. It is how I improve my prompts and recalibrate my trust over time.

## What I will _not_ delegate

- Final architectural decisions in ADRs
- Trust-boundary design
- Eval rubric design for AI labs
- Failure-mode enumeration on critical paths
- Naming conventions in shared packages
- The decision _not_ to use a tool

These are where my judgment compounds. The agent can draft; I decide.
