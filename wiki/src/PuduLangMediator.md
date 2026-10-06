---
type: module
path: "@root/src/PuduLangMediator.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.8
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangMediator]
---

# PuduLangMediator

## Purpose

The package root and its vocabulary: the `Failure` a mediated message answers instead of a value, the `Outcome` every handler, behavior, and processor answers, the `Violation` a validation failure lists, and the helpers that build, combine, inspect, and describe them.

## Interface

### Signatures

```pudu
export type Violation = { property: Str, message: Str, code: Str }

export type Failure[E]
  = Raised(E)
  | Unhandled(Str)
  | Mismatched(Str)
  | Refused(Str)
  | Invalid(Array[Violation])
  | Cancelled(Str)
  | Crashed(Str)
  | Aggregate(Array[Failure[E]])

export type Outcome[T, E] = Result[T, Failure[E]]

export fn succeed[T, E](value: T) -> Outcome[T, E]

export fn raise[T, E](problem: E) -> Outcome[T, E]

export fn refuse[T, E](reason: Str) -> Outcome[T, E]

export fn lift[T, E](result: Result[T, E]) -> Outcome[T, E]

export fn raised[E](failure: &Failure[E]) -> Option[E]

export fn isCancellation[E](failure: &Failure[E]) -> Bool

export fn violations[E](failure: &Failure[E]) -> Array[Violation]

export fn combine[E](failures: Array[Failure[E]]) -> Option[Failure[E]]

export fn describe[E](failure: &Failure[E]) -> Str

export fn summarize[T, E](outcome: &Outcome[T, E]) -> Str
```

### Linkage

- **Requires:** [[src/PuduLangMediator/Constants/Messages]], [[src/PuduLangMediator/Utils/Template]].
- **Consumed by:** [[src/PuduLangMediator/Behaviors/Logging]], [[src/PuduLangMediator/Behaviors/Resilient]], [[src/PuduLangMediator/Behaviors/Validation]], [[src/PuduLangMediator/Compose]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Mediator]], [[src/PuduLangMediator/Message]], [[src/PuduLangMediator/Notification]], [[src/PuduLangMediator/Open]], [[src/PuduLangMediator/Publisher]], [[src/PuduLangMediator/Publishing]], [[src/PuduLangMediator/Reader]], [[src/PuduLangMediator/Request]], [[src/PuduLangMediator/Sender]], [[src/PuduLangMediator/Stream]].

## Algorithm

1. `lift` maps a plain `Result` onto an outcome: `Ok` stays, `Err(e)` becomes `Raised(e)`.
2. `refuse` is how an open component fails: it cannot name the kind's `E`, so it answers `Refused` with a reason.
3. `combine` flattens nested `Aggregate`s: no failure is `None`, one is itself, several are one flat `Aggregate` in order.
4. `violations` reads the violations of an `Invalid`, including every `Invalid` inside an `Aggregate`.
5. `describe` fills one sentence per variant from [[src/PuduLangMediator/Constants/Messages]]; `summarize` renders a value as `Ok(<value>)` and a failure as its description.

## Negative Logic (Prohibited Paths)

- No wording is written inline; every sentence comes from the message constants.
- `combine` never nests an `Aggregate` inside another, so a caller matches one level.

## Edge Cases

- An `Aggregate` of one is never built: `combine` answers that one failure.
- `Raised` shows its error with `show`, so text errors appear quoted.

## Depth

DEPTH 0.8 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why one generic `Failure[E]` instead of the handler's own error type?
  **A:** Behaviors, processors, and the mediator itself must add their own failures (no handler, a refusal, an invalid message, a cancellation, a crash) without knowing `E`; one sum carries both and stays exhaustively matchable. _Rejected:_ a trait object of errors (loses exhaustive matching); one error type per layer (callers would unwrap one per layer).
- **Q:** Why `Refused` and `Invalid` as separate variants?
  **A:** `Invalid` carries structured violations a caller renders per field; `Refused` is the one failure an open component can build for any `E`. Folding either into the other loses the structure or the generality. _Rejected:_ a text-only `Invalid` (callers would parse messages back into fields).
- **Q:** Why `Aggregate`?
  **A:** Parallel, bounded, and continuing publication run every handler and must report every failure, not the first. _Rejected:_ answering only the first failure (hides the rest of a failed publication).

## Referenced by

[[CHANGELOG]] · [[decisions/ADR-0001-outcomes-not-exceptions]] · [[domain/Outcome]] · [[src/PuduLangMediator/Behaviors/Logging]] · [[src/PuduLangMediator/Behaviors/Resilient]] · [[src/PuduLangMediator/Behaviors/Validation]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Constants/Messages]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Message]] · [[src/PuduLangMediator/Notification]] · [[src/PuduLangMediator/Open]] · [[src/PuduLangMediator/Publisher]] · [[src/PuduLangMediator/Publishing]] · [[src/PuduLangMediator/Reader]] · [[src/PuduLangMediator/Request]] · [[src/PuduLangMediator/Sender]] · [[src/PuduLangMediator/Stream]] · [[src/PuduLangMediator/Utils/Template]] · [[src/_MOC]]
