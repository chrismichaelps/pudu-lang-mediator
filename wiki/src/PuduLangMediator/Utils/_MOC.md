---
type: moc
tags: [moc]
---

# PuduLangMediator.Utils

- [[src/PuduLangMediator/Utils/Erasure]] — Values carried without naming their type, read back type-safely. A `Packed` value is a closure that writes the value into the `Slot` it was packed through; `unpack` runs it against a slot and answers what arrived, so only the slot it was packed through ever reads it back.
- [[src/PuduLangMediator/Utils/Shared]] — A value shared between threads that changes only under its lock, so every read-modify-write is atomic.
- [[src/PuduLangMediator/Utils/Template]] — Fills the numbered slots of a message template in one pass.
- [[src/PuduLangMediator/Utils/Threads]] — The message a stopped thread left, used wherever a crash is reported as `Crashed`.

## Referenced by

[[src/PuduLangMediator/_MOC]]
