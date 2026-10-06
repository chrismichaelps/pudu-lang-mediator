---
type: seam
capacity: CRITICAL
tags: [seam]
---

# Publishing (seam)

## Classification

Strategy boundary between a publication and its handlers: `Publishing.Strategy` receives the
executors of one publication and answers its outcome ([[src/PuduLangMediator/Publishing]]).

## Adapters

- **Sequential** — the default; stops at the first failure.
- **Continuing**, **Parallel**, **Bounded** — every handler runs; failures are combined.
- **Custom** — any program type implementing the trait, set in `Mediator.Options`.

## Health

No strategy answers before every executor it started has finished, and crashes are values.

## Referenced by

[[domain/Publication]] · [[seams/_MOC]]
