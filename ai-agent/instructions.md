# AI Agent Instructions

This document tells any AI coding agent — including future-me using one — how to operate inside Playground.

## What this repo is

A long-term **engineering learning lab**, not a product.

The artifacts that matter most are: topic notes, ADRs, RFCs, interview notes, small labs, and concept-map links. Code is a means to better thinking, not the goal.

## Required reading before changes

1. [`README.md`](../README.md)
2. [`.ai-context.md`](../.ai-context.md)
3. [`coding-agent-rules.md`](coding-agent-rules.md)
4. [`docs/operating-system/learning-principles.md`](../docs/operating-system/learning-principles.md)
5. [`docs/operating-system/engineering-principles.md`](../docs/operating-system/engineering-principles.md)
6. [`docs/operating-system/topic-lifecycle.md`](../docs/operating-system/topic-lifecycle.md)
7. [`docs/operating-system/decision-making-framework.md`](../docs/operating-system/decision-making-framework.md)
8. [`docs/operating-system/review-checklists.md`](../docs/operating-system/review-checklists.md)
9. The relevant topic folder under [`docs/topics/`](../docs/topics/).

If those files contradict the user's request, surface the conflict — don't silently override.

## Working principles

1. **Do not blindly implement large systems.** First understand the topic, the goal, the constraints, and existing docs.
2. **Prefer small, reviewable changes.**
3. **Update relevant documentation when adding a lab or module.**
4. **Add ADRs for meaningful decisions.**
5. **Add failure-mode notes for non-trivial systems.**
6. **Add interview notes when the topic is interview-relevant.**
7. **Do not introduce heavyweight infrastructure without justification.**
8. **Do not use microservices unless the repo explicitly asks for it.**
9. **Do not add AI/LLM abstractions without an eval plan, logging plan, and failure-mode notes.**
10. **Prefer boring technology unless the lab is specifically about exploring alternatives.**
11. **Explain tradeoffs.** Every architectural choice should name alternatives and reversal triggers.
12. **Add TODOs as open questions, not vague comments.** Move durable ones to [`docs/concept-map/open-questions.md`](../docs/concept-map/open-questions.md).
13. **Never hide uncertainty.** "I am not sure" is a valid output.
14. **Any generated architecture should include alternatives and reversal triggers.**

## Suggested workflow

For any non-trivial task:

1. **Restate** the user's goal in your own words.
2. **Identify** the affected topic(s) under `docs/topics/` and read them.
3. **Plan**: list the smallest set of changes that achieves the goal. Prefer doc updates first.
4. **Run the [Decision-Making Framework](../docs/operating-system/decision-making-framework.md)** silently and surface anything that's missing.
5. **Execute** small, reviewable changes.
6. **Run the [relevant review checklist](../docs/operating-system/review-checklists.md)** before stopping.
7. **Log** notable outcomes via [`templates/ai-agent-review-template.md`](../templates/ai-agent-review-template.md).

## What to do when uncertain

- Ask the user.
- Or capture the uncertainty in [`docs/concept-map/open-questions.md`](../docs/concept-map/open-questions.md) and proceed with the smallest reversible step.
- Never paper over uncertainty with confident-sounding boilerplate.

## What to do when asked to do too much

- Say so. Suggest a smaller first slice. Name what is being deferred and why.
- Multiple small PRs > one mega-PR.

## What to never do

- Scaffold a full Next.js app, microservices stack, Kafka, or Kubernetes "just in case".
- Add LLM/agent abstractions without evals, logging, and failure-mode notes.
- Generate huge files of boilerplate that obscure the underlying concept.
- Suppress or hide uncertainty.
- Skip the topic notes and go straight to code on a non-trivial change.
- Make irreversible architectural decisions without explicit human review.
