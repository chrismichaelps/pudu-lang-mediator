---
type: module
path: "@root/src/PuduLangMediator/Stream.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangMediator.Stream]
---

# PuduLangMediator.Stream

## Purpose

Requests answered by a sequence of items: the typed `Kind[Q, T, E]`, the sink a handler emits into, stream behaviors that may wrap the sink, and envelopes, items, and answers for untyped streams.

## Interface

### Signatures

```pudu
export type Sink[T] = fn(T) -> Bool

export type Next[T, E] = fn(Context.Context, Sink[T]) -> Messaging.Outcome[(), E]

export type Handler[Q, T, E] = fn(Q, Context.Context, Sink[T]) -> Messaging.Outcome[(), E]

export type Behavior[Q, T, E] = fn(Q, Context.Context, Sink[T], Next[T, E]) -> Messaging.Outcome[(), E]

export type Route[Q, T, E] = { handlers: Array[Handler[Q, T, E]], behaviors: Array[Behavior[Q, T, E]] }

export type Kind[Q, T, E] = {
  info: Message.Info,
  routes: Erasure.Slot[Route[Q, T, E]],
  invokers: Erasure.Slot[Handler[Q, T, E]],
  requests: Erasure.Slot[Q],
  items: Erasure.Slot[T],
  outcomes: Erasure.Slot[Messaging.Outcome[(), E]]
}

export fn kind[Q, T, E](name: Str) -> Kind[Q, T, E]

export fn tagged[Q, T, E](subject: &Kind[Q, T, E], tags: Array[Str]) -> Kind[Q, T, E]

export fn route[Q, T, E]() -> Route[Q, T, E]

export fn each[T, E](items: &Array[T], sink: Sink[T]) -> Messaging.Outcome[(), E]

export fn envelope[Q, T, E](subject: &Kind[Q, T, E], request: Q) -> Message.Envelope

export fn itemOf[Q, T, E](subject: &Kind[Q, T, E], packed: &Erasure.Packed) -> Option[T]

export fn answerOf[Q, T, E](subject: &Kind[Q, T, E], answer: &Message.Answer) -> Option[Messaging.Outcome[(), E]]
```

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Message]], [[src/PuduLangMediator/Utils/Erasure]].
- **Consumed by:** [[src/PuduLangMediator/Behaviors/Logging]], [[src/PuduLangMediator/Behaviors/Validation]], [[src/PuduLangMediator/Compose]], [[src/PuduLangMediator/Mediator]], [[src/PuduLangMediator/Open]], [[src/PuduLangMediator/Reader]], [[src/PuduLangMediator/Registration]], [[src/PuduLangMediator/Sender]].

## Algorithm

1. A handler emits by calling the sink; the sink answers whether it wants another item.
2. A stream behavior runs `next` with the sink or one wrapping it, so it may filter, map, or count items.
3. `each` emits an array until the sink declines.

## Negative Logic (Prohibited Paths)

- A handler keeps emitting only while the sink answers `true`.

## Edge Cases

- A declined stream still succeeds: the consumer chose to stop.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why a push sink instead of a pull iterator?
  **A:** A push sink needs no thread, no buffer, and no state machine in the handler; pulling is offered separately by [[src/PuduLangMediator/Reader]], which runs the push stream on a thread behind a bounded channel. _Rejected:_ a pull iterator as the only form (every handler would be a state machine).

## Referenced by

[[CHANGELOG]] · [[domain/Kind]] · [[domain/Stream]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Behaviors/Logging]] · [[src/PuduLangMediator/Behaviors/Validation]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Message]] · [[src/PuduLangMediator/Open]] · [[src/PuduLangMediator/Reader]] · [[src/PuduLangMediator/Registration]] · [[src/PuduLangMediator/Sender]] · [[src/PuduLangMediator/Utils/Erasure]] · [[src/PuduLangMediator/_MOC]] · [[subsystems/Streams]]
