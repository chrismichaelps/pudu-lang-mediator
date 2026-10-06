---
type: module
path: "@root/src/PuduLangMediator/Publisher.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.4
depth_status: SHALLOW
tags: [module, seam]
aliases: [PuduLangMediator.Publisher]
---

# PuduLangMediator.Publisher

## Purpose

What a component that only publishes notifications depends on: a trait the mediator implements, so a test double stands in for it.

## Interface

### Signatures

```pudu
export trait Publisher {
  fn publish[N, E](self: &Self, subject: &Notification.Kind[N, E], notification: N, context: Context.Context) -> Messaging.Outcome[(), E]
  fn broadcast(self: &Self, envelope: Message.Envelope, context: Context.Context) -> Message.Answer
}
```

### Implementations

- `impl Publisher for Mediator.Mediator`

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Mediator]], [[src/PuduLangMediator/Message]], [[src/PuduLangMediator/Notification]].
- **Consumed by:** programs using the package.

## Algorithm

1. `Mediator.Mediator` implements it by delegating to `publishWith` and `broadcastWith`.

## Negative Logic (Prohibited Paths)

- A component holding a `dynamic Publisher` cannot send requests.

## Edge Cases

- A recording double lets a suite assert what was published.

## Depth

DEPTH 0.4 (SHALLOW). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why separate from the sender?
  **A:** Most components either ask or announce; separate traits keep each dependency as narrow as its use. _Rejected:_ one combined trait only.

## Referenced by

[[CHANGELOG]] · [[seams/Sender]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Message]] · [[src/PuduLangMediator/Notification]] · [[src/PuduLangMediator/_MOC]] · [[subsystems/Notifications]]
