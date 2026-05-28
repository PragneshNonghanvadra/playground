# scripts/create-interview-note

## Purpose

Create a new interview note under `docs/interviews/<area>/<slug>.md` from `templates/interview-template.md`.

## Status

Stub. Implement only when manual copy/paste of the template becomes painful.

## Proposed interface

```bash
# example, not yet implemented
node scripts/create-interview-note/index.js <args>
```

## Design notes

- Reads from `templates/`.
- Writes to the target path.
- Refuses to overwrite existing files.
- Prints what it created.

## When to implement

After the manual workflow has been used 5+ times and the friction is real.
