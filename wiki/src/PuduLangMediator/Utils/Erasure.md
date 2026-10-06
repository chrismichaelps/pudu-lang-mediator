---
type: module
path: "@root/src/PuduLangMediator/Utils/Erasure.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.9
depth_status: DEEP
tags: [module, leaf, backbone]
aliases: [PuduLangMediator.Utils.Erasure]
---

# PuduLangMediator.Utils.Erasure

## Purpose

Values carried without naming their type, read back type-safely. A `Packed` value is a closure that writes the value into the `Slot` it was packed through; `unpack` runs it against a slot and answers what arrived, so only the slot it was packed through ever reads it back.

## Interface

### Signatures

```pudu
export type Packed = fn() -> ()

export type Slot[T] = { lock: Sync.Mutex, held: Sync.Cell[Option[T]] }

export fn slot[T]() -> Slot[T]

export fn pack[T](target: &Slot[T], value: T) -> Packed

export fn unpack[T](source: &Slot[T], packed: &Packed) -> Option[T]
```

### Linkage

- **Requires:** standard library only.
- **Consumed by:** [[src/PuduLangMediator/Catalog]], [[src/PuduLangMediator/Compose]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Mediator]], [[src/PuduLangMediator/Message]], [[src/PuduLangMediator/Notification]], [[src/PuduLangMediator/Request]], [[src/PuduLangMediator/Stream]].

## Algorithm

1. `pack(slot, value)` captures the slot's cell and the value; running the closure stores `Some(value)` in that cell.
2. `unpack(slot, packed)` takes the slot's lock, clears the cell, runs the closure, and swaps the cell back to `None`, answering what it held.
3. A value packed through another slot writes another cell, so the reading slot still holds `None`.

## Negative Logic (Prohibited Paths)

- No cast exists anywhere: the type a value reads back as is the type its slot was made for.
- The cell is never read outside its lock, so two concurrent reads through one slot cannot see each other's value.

## Edge Cases

- A packed value reads back any number of times.
- Slots are runtime values; a `const` cannot hold one, which is why kinds and context keys are made once at startup.

## Depth

DEPTH 0.9 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why closures and cells instead of a dynamic value and a downcast?
  **A:** Pudu has no runtime type test or cast; `dynamic Trait` widens but never narrows. A slot gives every erased value exactly one typed reader, which is type-safe by construction. _Rejected:_ serializing messages to text (loses types and costs a round trip); one closed sum of every message (closes the set of kinds).
- **Q:** Why a lock per slot?
  **A:** Unpacking writes and then reads one shared cell; two threads unpacking through the same slot would otherwise read each other's value. The critical section is two cell operations. _Rejected:_ a fresh cell per unpack (the packed closure already captured the slot's cell).

## Referenced by

[[CHANGELOG]] · [[architecture/_MOC]] · [[decisions/ADR-0002-typed-kinds]] · [[domain/Erasure]] · [[grammar/pudu]] · [[src/PuduLangMediator/Catalog]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Message]] · [[src/PuduLangMediator/Notification]] · [[src/PuduLangMediator/Request]] · [[src/PuduLangMediator/Stream]] · [[src/PuduLangMediator/Utils/_MOC]]
