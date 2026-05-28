# scripts/create-topic

## Purpose

Create a new topic folder under `docs/topics/<category>/<slug>/` with all 17 standard files seeded from `templates/topic-template.md`.

## Status

Stub. Implement only when manual copy/paste of the template becomes painful.

## Proposed interface

```bash
# example, not yet implemented
node scripts/create-topic/index.js <args>
```

## Design notes

- Reads from `templates/`.
- Writes to the target path.
- Refuses to overwrite existing files.
- Prints what it created.

## When to implement

After the manual workflow has been used 5+ times and the friction is real.
