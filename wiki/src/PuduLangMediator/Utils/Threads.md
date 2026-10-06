---
type: module
path: "@root/src/PuduLangMediator/Utils/Threads.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.3
depth_status: SHALLOW
tags: [module, leaf]
aliases: [PuduLangMediator.Utils.Threads]
---

# PuduLangMediator.Utils.Threads

## Purpose

The message a stopped thread left, used wherever a crash is reported as `Crashed`.

## Interface

### Signatures

```pudu
export fn reasonOf(problem: &Concurrent.ConcurrentError) -> Str
```

### Linkage

- **Requires:** standard library only.
- **Consumed by:** [[src/PuduLangMediator/Publishing]], [[src/PuduLangMediator/Reader]].

## Algorithm

1. `Failed(said)` answers `said`; any other error answers its `show`.

## Negative Logic (Prohibited Paths)

- A crash is never reported as the error's constructor text when it carries a message.

## Edge Cases

- `Missing` and other errors without a message are shown whole.

## Depth

DEPTH 0.3 (SHALLOW). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why a module for one function?
  **A:** Publishing and the stream reader both report crashed threads; one definition keeps their wording identical. _Rejected:_ a copy in each module.

## Referenced by

[[src/PuduLangMediator/Publishing]] · [[src/PuduLangMediator/Reader]] · [[src/PuduLangMediator/Utils/_MOC]]
