---
type: module
path: "@root/src/PuduLangMediator/Sender.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.4
depth_status: SHALLOW
tags: [module, seam]
aliases: [PuduLangMediator.Sender]
---

# PuduLangMediator.Sender

## Purpose

What a component that only sends requests and streams depends on: a trait the mediator implements, so a test double stands in for it.

## Interface

### Signatures

```pudu
export trait Sender {
  fn send[Q, R, E](self: &Self, subject: &Request.Kind[Q, R, E], request: Q, context: Context.Context) -> Messaging.Outcome[R, E]
  fn dispatch(self: &Self, envelope: Message.Envelope, context: Context.Context) -> Message.Answer
  fn stream[Q, T, E](self: &Self, subject: &Stream.Kind[Q, T, E], request: Q, context: Context.Context, sink: Stream.Sink[T]) -> Messaging.Outcome[(), E]
}
```

### Implementations

- `impl Sender for Mediator.Mediator`

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Mediator]], [[src/PuduLangMediator/Message]], [[src/PuduLangMediator/Request]], [[src/PuduLangMediator/Stream]].
- **Consumed by:** programs using the package.

## Algorithm

1. `Mediator.Mediator` implements it by delegating to `sendWith`, `dispatchWith`, and `streamWith`.

## Negative Logic (Prohibited Paths)

- A component holding a `dynamic Sender` cannot publish.

## Edge Cases

- A double may answer anything without a mediator.

## Depth

DEPTH 0.4 (SHALLOW). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why a trait and not the mediator type itself?
  **A:** Narrow dependencies document what a component does and make it testable without building a mediator. _Rejected:_ passing the mediator everywhere.

## Referenced by

[[CHANGELOG]] · [[seams/Sender]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Message]] · [[src/PuduLangMediator/Request]] · [[src/PuduLangMediator/Stream]] · [[src/PuduLangMediator/_MOC]] · [[subsystems/Requests]]
