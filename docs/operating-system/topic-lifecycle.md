# Topic Lifecycle

Every topic in this repo can move through these stages. A topic does not need to reach Stage 12 to be valuable. Many topics will live happily at Stage 4 or Stage 6.

The lifecycle is a _menu of progress_, not a checklist of obligations.

---

## Stage 0: Curiosity

**What to write**

- One sentence in [`docs/concept-map/learning-backlog.md`](../concept-map/learning-backlog.md) describing the topic and why it caught my attention.

**What to build**

- Nothing.

**Questions to ask**

- What problem is this trying to solve?
- Who already cares about this and why?

**Output in repo**

- An entry in the learning backlog.

---

## Stage 1: First-principles understanding

**What to write**

- A draft `why-it-matters.md` — explain the problem **without naming the popular tool**.

**What to build**

- Nothing yet.

**Questions to ask**

- What would I have to invent if no tool existed?
- What forces created this category in the first place?

**Output**

- `docs/topics/<area>/<topic>/why-it-matters.md` filled out.

---

## Stage 2: Mental model

**What to write**

- `mental-model.md` — the simplest durable picture, ideally one paragraph plus a sketch.

**Questions to ask**

- Can I explain this in 30 seconds?
- What metaphor survives scrutiny?
- What does the model _not_ explain?

**Output**

- `mental-model.md` complete.

---

## Stage 3: Real-world usage

**What to write**

- `must-know.md` — the irreducible vocabulary and patterns.
- `good-to-know.md` — adjacent things to recognize.

**Questions to ask**

- Where does this show up in production systems I admire?
- What three concrete companies / projects rely on this and how?

**Output**

- `must-know.md` and `good-to-know.md` complete with concrete examples.

---

## Stage 4: Tradeoffs and alternatives

**What to write**

- `tradeoffs.md` — at least three options, with pros/cons and "when to choose".

**Questions to ask**

- What does each option force me to commit to?
- What does each option _prevent_?
- What is the default and why?

**Output**

- `tradeoffs.md` complete.
- Optional: a `tradeoff-template.md` instance under `docs/architecture/tradeoffs/`.

---

## Stage 5: Failure modes

**What to write**

- `failure-modes.md` — concrete ways this fails.
- `beginner-mistakes.md` — common shallow misunderstandings.

**Questions to ask**

- How does this fail under partial failure?
- How does this fail under load?
- How does this fail under bad input?
- How does this fail in the hands of a junior engineer?

**Output**

- `failure-modes.md` and `beginner-mistakes.md` complete.

---

## Stage 6: Tiny experiment

**What to build**

- A lab in `labs/<area>/<lab-name>/` with a `README.md` based on [`templates/lab-template.md`](../../templates/lab-template.md).
- The lab must include a measurement and a failure experiment.

**Output**

- A working lab + an entry in `experiments.md` linking to it.

---

## Stage 7: Production concerns

**What to write**

- `production-concerns.md` — what would change to deploy this for real users.
- Optionally a `production-readiness-template.md` instance for the lab.

**Questions to ask**

- Auth? Authz? Tenant isolation?
- Retries, idempotency, timeouts?
- Backups, migrations, rollback?
- Observability?

**Output**

- `production-concerns.md` complete.

---

## Stage 8: Scaling implications

**What to write**

- `scaling-considerations.md` — what changes at 10x, 100x, 1000x.

**Questions to ask**

- What breaks first?
- What is the bottleneck?
- What is the next architectural step (and the one after)?

**Output**

- `scaling-considerations.md` complete.

---

## Stage 9: Interview explanation

**What to write**

- `interview-notes.md` — 30-second, 2-minute, deep-dive explanations.
- An entry under `docs/interviews/<area>/`.

**Questions to ask**

- What follow-ups would an interviewer ask?
- What tradeoffs do strong answers always name?

**Output**

- `interview-notes.md` complete and a corresponding interview file.

---

## Stage 10: Connected concepts

**What to write**

- `connected-concepts.md` — links to related topic notes.
- Update [`docs/concept-map/connections.md`](../concept-map/connections.md).

**Questions to ask**

- Where does this concept compose with others?
- Where do these concepts conflict?

**Output**

- Topic appears in the concept map graph.

---

## Stage 11: Reusable module

**What to build**

- A small reusable package under `packages/` if appropriate.
- An ADR under `docs/architecture/adr/` if a meaningful decision was made.

**Output**

- A package or module that another lab can import.

---

## Stage 12: Product integration

**What to build**

- The module is used in a real product slice across `apps/` and `services/`.
- A production-readiness doc exists.
- An ADR captures the cross-cutting decision.

**Output**

- Working slice + production-readiness doc + ADR + lessons captured back into the topic notes.

---

## Stop conditions

It is **fine** to stop a topic at any stage. The lifecycle is for ambition, not pressure.

What's _not_ fine:

- Skipping Stages 1–5 to jump straight to code (I'm building muscles, not shipping product).
- Stopping at Stage 6 (a lab) without writing back the lessons into the topic notes.
- Reaching Stage 12 without writing an ADR.
