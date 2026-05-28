# Review Checklists

Run the relevant checklist before considering work "done". These are deliberately repetitive across categories — the point is to catch myself when I'm rushing.

---

## Topic note checklist

Before marking a topic note "stable":

- [ ] `why-it-matters.md` explains the problem without naming the tool first.
- [ ] `mental-model.md` is one paragraph or less + an optional sketch.
- [ ] `must-know.md` lists the irreducible vocabulary.
- [ ] `tradeoffs.md` names at least three options.
- [ ] `failure-modes.md` lists at least three concrete failures.
- [ ] `interview-notes.md` includes 30s, 2m, deep-dive.
- [ ] `connected-concepts.md` has at least 2 cross-links.
- [ ] [`docs/concept-map/connections.md`](../concept-map/connections.md) has been updated.

---

## Lab checklist

Before merging a lab:

- [ ] `README.md` follows [`templates/lab-template.md`](../../templates/lab-template.md).
- [ ] Scope and non-goals are explicit.
- [ ] At least one measurement is taken.
- [ ] At least one failure experiment is documented.
- [ ] Production notes section is filled (even if "this is not production-ready because…").
- [ ] AI-agent notes section names what an agent could miss.
- [ ] Lessons are written back to the topic notes.

---

## ADR checklist

Before marking an ADR "Accepted":

- [ ] At least three alternatives are listed.
- [ ] Each alternative has pros, cons, and a "why not chosen" rationale.
- [ ] Reversal triggers are concrete (e.g., metric thresholds), not vague.
- [ ] Linked from at least one topic note and one lab (if applicable).

---

## RFC checklist

Before marking an RFC ready for review:

- [ ] Goals and non-goals are listed.
- [ ] Failure modes are enumerated.
- [ ] Observability is specified (logs, metrics, traces, alerts).
- [ ] Security section addresses trust boundaries.
- [ ] Performance section has numeric targets.
- [ ] Rollout plan includes rollback.
- [ ] Open questions are explicit, not buried.

---

## Production-readiness checklist

Before any module is allowed near real users:

- [ ] Owner is named.
- [ ] Auth and authz are defined.
- [ ] Inputs are validated at trust boundaries.
- [ ] Operations are idempotent (or duplicates are explicitly safe).
- [ ] Retries have backoff and idempotency keys.
- [ ] Timeouts exist at every network boundary.
- [ ] Errors are structured and observable.
- [ ] Logs are structured and PII-aware.
- [ ] Metrics include rate, errors, duration.
- [ ] Traces span entry to exit.
- [ ] SLO / SLI defined.
- [ ] Backups and migrations have rollback plans.
- [ ] Secrets are not in source.
- [ ] Multi-tenancy is enforced (if applicable).
- [ ] Runbooks exist for the top 3 incidents.

---

## AI-eval checklist

Before any AI feature affects users:

- [ ] Eval dataset exists and is versioned.
- [ ] Rubric is documented.
- [ ] Regression suite runs in CI or scheduled.
- [ ] Prompts are stored under version control.
- [ ] Per-call tokens, latency, and cost are logged.
- [ ] Output is validated (schema, allowlist, or rubric).
- [ ] Unsafe actions require human approval.
- [ ] Prompt-injection vectors have been considered.

---

## AI Agent Review Checklist

Run this when reviewing AI-generated changes.

### Architecture

- What is the source of truth?
- What state is durable?
- What state is cached?
- What can fail independently?
- What are the consistency requirements?
- What is the rollback path?

### Backend

- Are inputs validated?
- Are permissions checked?
- Are operations idempotent?
- Are transactions needed?
- Are errors handled intentionally?

### AI

- Is there an eval plan?
- Is prompt/model usage logged?
- Is output validated?
- Are unsafe actions gated?
- Is cost measured?

### Data

- Is schema ownership clear?
- Are indexes needed?
- Are migrations reversible?
- Is sensitive data protected?

### Observability

- Are logs structured?
- Are metrics defined?
- Are traces needed?
- Can this be debugged in production?

### Security

- What is the trust boundary?
- Can tenant data leak?
- Can user input become instruction?
- Are secrets protected?

### Product

- What user problem does this solve?
- What is the smallest useful version?
- What feedback should be captured?
