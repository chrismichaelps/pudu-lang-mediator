---
type: module
path: "@root/src/PuduLangMediator/Utils/Template.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MEDIUM
tags: [module, leaf]
aliases: [PuduLangMediator.Utils.Template]
---

# PuduLangMediator.Utils.Template

## Purpose

Fills the numbered slots of a message template in one pass.

## Interface

### Signatures

```pudu
export fn fill(template: Str, values: &Array[Str]) -> Str
```

### Linkage

- **Requires:** standard library only.
- **Consumed by:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Domain/Census]], [[src/PuduLangMediator/Mediator]].

## Algorithm

1. Scan for `<`; when the text up to the next `>` is a number with a value, substitute it; otherwise keep the text as written.
2. Text taken from a value is never scanned again.

## Negative Logic (Prohibited Paths)

- A value containing `<1>` is not filled a second time.

## Edge Cases

- A slot without a value is kept as written, so a missing value is visible rather than empty.

## Depth

DEPTH 0.6 (MEDIUM). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why not repeated `replace` calls?
  **A:** A value that itself contains a slot would be filled again by a later replace; one pass cannot do that. _Rejected:_ `Text.replace` per slot.

## Referenced by

[[src/PuduLangMediator]] · [[src/PuduLangMediator/Constants/Messages]] · [[src/PuduLangMediator/Constants/_MOC]] · [[src/PuduLangMediator/Domain/Census]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Utils/_MOC]]
