# apps/

Placeholder for future user-facing applications. These are intentionally empty until accumulated lab work justifies a real product slice.

## Subfolders

- [`web/`](web/) — primary user-facing web app
- [`admin/`](admin/) — internal / admin UI
- [`docs-site/`](docs-site/) — docs site (Docusaurus / Astro / Nextra), if needed

## Quality gate

Before spinning up a real app here:

1. There is an ADR explaining why a product slice is justified.
2. The slice integrates modules from `packages/` that have graduated from labs.
3. The slice has a production-readiness checklist (see [`templates/production-readiness-template.md`](../templates/production-readiness-template.md)).
