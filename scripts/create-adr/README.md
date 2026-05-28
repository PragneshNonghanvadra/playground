# scripts/create-adr

## Purpose

Create a new ADR under `docs/architecture/adr/NNNN-<slug>.md`, auto-numbering.

## Status

Stub. Implement only when manual copy/paste of the template becomes painful.

## Proposed interface

```bash
# example, not yet implemented
node scripts/create-adr/index.js <args>
```

## Design notes

- Reads from `templates/`.
- Writes to the target path.
- Refuses to overwrite existing files.
- Prints what it created.

## When to implement

After the manual workflow has been used 5+ times and the friction is real.
