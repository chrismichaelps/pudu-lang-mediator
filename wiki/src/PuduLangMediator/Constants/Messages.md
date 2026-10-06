---
type: module
path: "@root/src/PuduLangMediator/Constants/Messages.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.3
depth_status: SHALLOW
tags: [module, leaf, constants]
aliases: [PuduLangMediator.Constants.Messages]
---

# PuduLangMediator.Constants.Messages

## Purpose

Every sentence the package reports, as numbered templates filled by [[src/PuduLangMediator/Utils/Template]].

## Interface

### Signatures

```pudu
export const RAISED: Str

export const UNHANDLED: Str

export const MISMATCHED: Str

export const REFUSED: Str

export const INVALID: Str

export const VIOLATION: Str

export const CANCELLED: Str

export const CRASHED: Str

export const AGGREGATE: Str

export const SUCCEEDED: Str

export const MEDIATOR_INVALID: Str

export const EMPTY_NAME: Str

export const NAME_TAKEN: Str

export const DUPLICATE_HANDLER: Str

export const WITHOUT_HANDLER: Str

export const READER_CLOSED: Str

export const SEPARATOR: Str
```

### Linkage

- **Requires:** standard library only.
- **Consumed by:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Domain/Census]], [[src/PuduLangMediator/Mediator]], [[src/PuduLangMediator/Reader]].

## Algorithm

1. Each template names its slots in its doc comment; `<1>` is the first value.

## Negative Logic (Prohibited Paths)

- No template is built by concatenation elsewhere; a new sentence is a new constant here.

## Edge Cases

- Problems listed in one sentence are joined with `SEPARATOR`.

## Depth

DEPTH 0.3 (SHALLOW). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why keep every sentence in one module?
  **A:** Tests and callers compare descriptions; one place to read and change them keeps wording consistent and reviewable. _Rejected:_ inline strings in each module (drift between similar sentences).

## Referenced by

[[src/PuduLangMediator]] · [[src/PuduLangMediator/Constants/_MOC]] · [[src/PuduLangMediator/Domain/Census]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Reader]]
