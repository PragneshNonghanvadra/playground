# scripts/create-rfc

## Purpose

Create a new RFC under `docs/architecture/rfcs/NNNN-<slug>.md`, auto-numbering.

## Status

Stub. Implement only when manual copy/paste of the template becomes painful.

## Proposed interface

```bash
# example, not yet implemented
node scripts/create-rfc/index.js <args>
```

## Design notes

- Reads from `templates/`.
- Writes to the target path.
- Refuses to overwrite existing files.
- Prints what it created.

## When to implement

After the manual workflow has been used 5+ times and the friction is real.
