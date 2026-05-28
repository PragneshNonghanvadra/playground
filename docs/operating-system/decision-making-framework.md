# Decision-Making Framework

Before choosing a tool or architecture, walk through these questions. The point is not to fill them all in — it's to **notice when one is being skipped**.

For meaningful decisions, capture the answers in an ADR using [`templates/adr-template.md`](../../templates/adr-template.md).

## Problem

- What problem are we solving?
- Who experiences this problem?
- What happens if we do nothing?
- Have we confirmed the problem actually exists, or are we anticipating?

## Constraints

- What are the latency constraints?
- What are the reliability constraints?
- What are the cost constraints?
- What are the team / complexity constraints?
- What are the time constraints (deadline, runway, calendar)?

## Data

- What is the source of truth?
- What data is read often? What data is written often?
- What consistency is required (strong, read-your-writes, eventual)?
- What is the access pattern?
- How is sensitive data identified and protected?

## Failure

- What can fail?
- What happens during partial failure?
- What needs retry? What backoff?
- What must be idempotent?
- What is the blast radius?
- What is the rollback path?

## Scale

- What is the simplest version that proves the idea?
- What breaks first as load grows?
- What is the next evolution step? And the one after that?
- What is the cost shape (linear, sublinear, superlinear)?

## Operations

- How will this be deployed?
- How will this be monitored?
- How will this be rolled back?
- Who is on call?
- What's the smallest reproducible local environment?

## Security

- What is the trust boundary?
- What can be abused?
- What data must be protected?
- For AI: can user input become instruction (prompt injection)? Can outputs trigger actions without review?

## Alternatives

- What are at least three options?
- Why reject the others?
- What would make us change our mind (reversal triggers)?

## Reversibility

- If we're wrong, how hard is it to undo?
- Is this a one-way door or a two-way door?
- Two-way door decisions: bias toward action.
- One-way door decisions: bias toward more analysis, smaller bets, optionality.

## People

- Who maintains this in 6 months?
- Who maintains this in 2 years?
- What does this require the team to _learn_?
- What does this require the team to _operate_?

## Sanity checks

- Am I choosing this because it's the right tool, or because it's the _new_ tool?
- Am I designing for actual scale, or for hypothetical scale?
- Am I solving the user's problem, or my own architectural taste?
- Could I justify this decision to a skeptical senior engineer in three sentences?
