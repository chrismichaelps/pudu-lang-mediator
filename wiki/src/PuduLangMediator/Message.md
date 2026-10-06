---
type: module
path: "@root/src/PuduLangMediator/Message.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MEDIUM
tags: [module, backbone]
aliases: [PuduLangMediator.Message]
---

# PuduLangMediator.Message

## Purpose

What every kind of message shares: its `Info` (name, shape, tags), the `Envelope` a sender that names no types sends, and the `Answer` it gets back.

## Interface

### Signatures

```pudu
export type Shape = Query | Command | Event | Sequence

export type Info = { name: Str, shape: Shape, tags: Array[Str] }

export type Envelope = { info: Info, packed: Erasure.Packed, summary: Str }

export type Fault = NoRoute(Str) | WrongKind(Str)

export type Delivery = Delivered(Erasure.Packed) | Undelivered(Fault) | Unheard

export type Answer = { info: Info, delivery: Delivery, failed: Bool, summary: Str }

export fn hasTag(info: &Info, tag: Str) -> Bool

export fn shapeName(shape: &Shape) -> Str

export fn failureOf[E](fault: &Fault) -> Messaging.Failure[E]

export fn answered[T, E](info: &Info, outcomes: &Erasure.Slot[Messaging.Outcome[T, E]], outcome: Messaging.Outcome[T, E]) -> Answer

export fn undelivered(info: &Info, fault: Fault) -> Answer

export fn unheard(info: &Info) -> Answer

export fn read[T, E](outcomes: &Erasure.Slot[Messaging.Outcome[T, E]], answer: &Answer) -> Option[Messaging.Outcome[T, E]]

export fn sealed[M](info: &Info, messages: &Erasure.Slot[M], message: M) -> Envelope
```

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Utils/Erasure]].
- **Consumed by:** [[src/PuduLangMediator/Behaviors/Logging]], [[src/PuduLangMediator/Catalog]], [[src/PuduLangMediator/Compose]], [[src/PuduLangMediator/Mediator]], [[src/PuduLangMediator/Notification]], [[src/PuduLangMediator/Open]], [[src/PuduLangMediator/Publisher]], [[src/PuduLangMediator/Request]], [[src/PuduLangMediator/Sender]], [[src/PuduLangMediator/Stream]].

## Algorithm

1. `sealed` packs a message through its kind's message slot and keeps its `show` as the summary.
2. `answered` packs an outcome through its kind's outcome slot and records whether it failed and how it reads.
3. `read` unpacks an answer's outcome; an undelivered answer reads as `Unhandled` or `Mismatched` for any type; an unheard one reads as nothing here and as success through its notification kind.

## Negative Logic (Prohibited Paths)

- An answer never reads back through another kind's slot.

## Edge Cases

- A notification envelope whose kind has no handler is `Unheard`: not failed, summarized `Ok(())`.

## Depth

DEPTH 0.6 (MEDIUM). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why separate `Unheard` from `Undelivered`?
  **A:** Publishing to nobody succeeds, sending to nobody fails; one variant could not answer both truthfully. _Rejected:_ treating an unheard publication as unhandled (a quiet event would look like an error).

## Referenced by

[[domain/Erasure]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Behaviors/Logging]] · [[src/PuduLangMediator/Catalog]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Notification]] · [[src/PuduLangMediator/Open]] · [[src/PuduLangMediator/Publisher]] · [[src/PuduLangMediator/Request]] · [[src/PuduLangMediator/Sender]] · [[src/PuduLangMediator/Stream]] · [[src/PuduLangMediator/Utils/Erasure]] · [[src/PuduLangMediator/_MOC]]
