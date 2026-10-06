---
type: module
path: "@root/src/PuduLangMediator/Utils/Shared.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, leaf]
aliases: [PuduLangMediator.Utils.Shared]
---

# PuduLangMediator.Utils.Shared

## Purpose

A value shared between threads that changes only under its lock, so every read-modify-write is atomic.

## Interface

### Signatures

```pudu
export type Shared[S] = { lock: Sync.Mutex, held: Sync.Cell[S] }

export fn shared[S](initial: S) -> Shared[S]

export fn current[S](source: &Shared[S]) -> S

export fn change[S, R](target: &Shared[S], step: fn(S) -> (S, R)) -> R

export fn update[S](target: &Shared[S], step: fn(S) -> S) -> ()
```

### Linkage

- **Requires:** standard library only.
- **Consumed by:** [[src/PuduLangMediator/Behaviors/Logging]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Mediator]].

## Algorithm

1. `change` reads, computes, and writes under the mutex and answers the second half of what the step returned.
2. `update` is `change` with no answer.

## Negative Logic (Prohibited Paths)

- No write depends on a read made outside the lock.

## Edge Cases

- A poisoned lock or cell stops the program: shared state that cannot be read is not recoverable here.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why a mutex around the cell rather than the cell alone?
  **A:** A cell makes single reads and writes atomic, not a read followed by a write; concurrent updates would be lost. _Rejected:_ a bare `Sync.Cell` (lost updates under contention).

## Referenced by

[[grammar/pudu]] · [[src/PuduLangMediator/Behaviors/Logging]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Utils/_MOC]]
