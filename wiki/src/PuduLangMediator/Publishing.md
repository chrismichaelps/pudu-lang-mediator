---
type: module
path: "@root/src/PuduLangMediator/Publishing.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.8
depth_status: DEEP
tags: [module, seam]
aliases: [PuduLangMediator.Publishing]
---

# PuduLangMediator.Publishing

## Purpose

How a notification reaches its handlers: the `Strategy` trait over executors and four strategies — sequential, continuing, parallel, and bounded.

## Interface

### Signatures

```pudu
export type Executor[E] = { handler: Str, run: fn(Context.Context) -> Messaging.Outcome[(), E] }

export trait Strategy {
  fn publish[E](self: &Self, executors: &Array[Executor[E]], context: Context.Context) -> Messaging.Outcome[(), E]
}

export type Sequential = {}

export type Continuing = {}

export type Parallel = {}

export type Bounded = { workers: Int }
```

### Implementations

- `impl Strategy for Sequential`
- `impl Strategy for Continuing`
- `impl Strategy for Parallel`
- `impl Strategy for Bounded`

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Utils/Threads]].
- **Consumed by:** [[src/PuduLangMediator/Catalog]], [[src/PuduLangMediator/Compose]], [[src/PuduLangMediator/Mediator]].

## Algorithm

1. `Sequential` checks the context, then runs each executor in order, and answers the first failure; later handlers never run.
2. `Continuing` runs every executor in order and answers every failure, combined.
3. `Parallel` starts every executor on its own thread, joins all, and answers every failure in executor order; a thread that stops is `Crashed`.
4. `Bounded` runs every executor with at most `workers` at once (at least one), each contained so a crash is reported and the rest still run.

## Negative Logic (Prohibited Paths)

- No strategy answers before every executor it started has finished.
- No strategy drops a failure; parallel and bounded report crashes as values.

## Edge Cases

- No executors succeed at once.
- A bounded strategy asked for zero workers runs one at a time.

## Depth

DEPTH 0.8 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why is sequential the default?
  **A:** It runs handlers on the caller's thread in registration order and stops at the first failure, the least surprising behavior; the others are chosen explicitly. _Rejected:_ parallel by default (surprising for handlers that share state).
- **Q:** Why add continuing and bounded?
  **A:** Continuing is sequential without losing later handlers to an early failure; bounded caps the threads a large fan-out starts. _Rejected:_ only the two common strategies (callers would write these again).

## Referenced by

[[CHANGELOG]] · [[domain/Publication]] · [[seams/Publishing]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Catalog]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Utils/Threads]] · [[src/PuduLangMediator/_MOC]] · [[subsystems/Notifications]]
