# evals/

Datasets, rubrics, and regression suites for AI work.

## Subfolders

- [`datasets/`](datasets/) — golden Q/A sets, labeled examples
- [`rubrics/`](rubrics/) — evaluation rubrics (faithfulness, helpfulness, safety)
- [`regression-suites/`](regression-suites/) — eval runs to detect drift

## Principles

- An AI feature without evals is theater.
- Datasets are versioned. Changes to a dataset require a note.
- Rubrics are explicit. "Looks good to me" is not a rubric.
- Regression suites run on a schedule or in CI.
