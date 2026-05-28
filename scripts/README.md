# scripts/

Placeholder folder for repo-management scripts. **Keep these minimal.** Most can stay as documented manual workflows until they're truly worth automating.

## Planned scripts

- [`create-topic/`](create-topic/) — scaffold a new topic folder from [`templates/topic-template.md`](../templates/topic-template.md)
- [`create-lab/`](create-lab/) — scaffold a new lab folder from [`templates/lab-template.md`](../templates/lab-template.md)
- [`create-adr/`](create-adr/) — scaffold a numbered ADR from [`templates/adr-template.md`](../templates/adr-template.md)
- [`create-rfc/`](create-rfc/) — scaffold a numbered RFC from [`templates/rfc-template.md`](../templates/rfc-template.md)
- [`create-interview-note/`](create-interview-note/) — scaffold an interview note
- [`create-resource-note/`](create-resource-note/) — scaffold a resource note

## Design principles

- Each script should do **one thing** and be readable in one screen.
- No flag-heavy CLIs. Three args max.
- Idempotent: never overwrite existing files.
- Prefer Node.js or POSIX shell. Avoid Python unless a lab requires it.

## Status

All scripts are currently stubs. See each subfolder's README for the planned interface.
