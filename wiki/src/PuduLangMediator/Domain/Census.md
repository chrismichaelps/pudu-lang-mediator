---
type: module
path: "@root/src/PuduLangMediator/Domain/Census.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MEDIUM
tags: [module, domain, pure]
aliases: [PuduLangMediator.Domain.Census]
---

# PuduLangMediator.Domain.Census

## Purpose

What a registered kind is missing or has twice: a census of its handlers, components, and name collisions answers every problem at once.

## Interface

### Signatures

```pudu
export type Census = { name: Str, handlers: Int, components: Int, collisions: Int, single: Bool }

export fn problems(census: &Census) -> Array[Str]
```

### Linkage

- **Requires:** [[src/PuduLangMediator/Constants/Messages]], [[src/PuduLangMediator/Utils/Template]].
- **Consumed by:** [[src/PuduLangMediator/Catalog]].

## Algorithm

1. An empty name is a problem.
2. Any collision (another kind registered under the same name) is a problem.
3. A request or stream kind (`single`) with more than one handler, or with components but no handler, is a problem.

## Negative Logic (Prohibited Paths)

- A notification kind may have any number of handlers, including none.

## Edge Cases

- Every problem of one census is reported together, in the order above.

## Depth

DEPTH 0.6 (MEDIUM). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why refuse a second handler instead of letting the last one win?
  **A:** A request answered by whichever handler registered last depends on registration order nobody reads; refusing it at build time surfaces the mistake once. _Rejected:_ last registration wins (silent shadowing).

## Referenced by

[[decisions/ADR-0004-explicit-registration]] · [[src/PuduLangMediator/Catalog]] · [[src/PuduLangMediator/Constants/Messages]] · [[src/PuduLangMediator/Domain/_MOC]] · [[src/PuduLangMediator/Utils/Template]]
