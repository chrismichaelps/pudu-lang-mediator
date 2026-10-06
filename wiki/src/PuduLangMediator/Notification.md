---
type: module
path: "@root/src/PuduLangMediator/Notification.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MEDIUM
tags: [module, backbone]
aliases: [PuduLangMediator.Notification]
---

# PuduLangMediator.Notification

## Purpose

Notifications delivered to every handler: the typed `Kind[N, E]`, named handlers, the `Route` that gathers them, and envelopes and answers.

## Interface

### Signatures

```pudu
export type Handler[N, E] = fn(N, Context.Context) -> Messaging.Outcome[(), E]

export type Named[N, E] = { name: Str, handle: Handler[N, E] }

export type Route[N, E] = { handlers: Array[Named[N, E]] }

export type Kind[N, E] = {
  info: Message.Info,
  routes: Erasure.Slot[Route[N, E]],
  invokers: Erasure.Slot[Handler[N, E]],
  notifications: Erasure.Slot[N],
  outcomes: Erasure.Slot[Messaging.Outcome[(), E]]
}

export fn kind[N, E](name: Str) -> Kind[N, E]

export fn tagged[N, E](subject: &Kind[N, E], tags: Array[Str]) -> Kind[N, E]

export fn route[N, E]() -> Route[N, E]

export fn envelope[N, E](subject: &Kind[N, E], notification: N) -> Message.Envelope

export fn answerOf[N, E](subject: &Kind[N, E], answer: &Message.Answer) -> Option[Messaging.Outcome[(), E]]
```

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Message]], [[src/PuduLangMediator/Utils/Erasure]].
- **Consumed by:** [[src/PuduLangMediator/Compose]], [[src/PuduLangMediator/Mediator]], [[src/PuduLangMediator/Publisher]], [[src/PuduLangMediator/Registration]].

## Algorithm

1. A handler is registered with the name publishing strategies report it by.
2. `answerOf` reads an unheard publication of this kind as success.

## Negative Logic (Prohibited Paths)

- An unheard answer of another kind is not read as this kind's success.

## Edge Cases

- A kind may have no handlers; publishing it still runs the open handlers.

## Depth

DEPTH 0.6 (MEDIUM). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why name notification handlers?
  **A:** A publishing strategy reports and may treat handlers by name (an executor), and diagnostics need to say which handler failed. _Rejected:_ anonymous handlers.

## Referenced by

[[CHANGELOG]] · [[domain/Kind]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Message]] · [[src/PuduLangMediator/Publisher]] · [[src/PuduLangMediator/Registration]] · [[src/PuduLangMediator/Utils/Erasure]] · [[src/PuduLangMediator/_MOC]] · [[subsystems/Notifications]]
