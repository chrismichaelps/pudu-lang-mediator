---
type: module
path: "@root/src/PuduLangMediator/Reader.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.8
depth_status: DEEP
tags: [module]
aliases: [PuduLangMediator.Reader]
---

# PuduLangMediator.Reader

## Purpose

A stream read one item at a time while another thread produces it, holding a bounded number of items ahead of the consumer.

## Interface

### Signatures

```pudu
export type Step[T, E] = Item(T) | Finished(Messaging.Outcome[(), E])

export type Reader[T, E] = {
  channel: Channel.Channel[Step[T, E]],
  context: Context.Context,
  worker: Option[Concurrent.Task],
  done: Sync.Cell[Bool]
}

export fn open[T, E](capacity: Int, context: &Context.Context, produce: fn(Context.Context, Stream.Sink[T]) -> Messaging.Outcome[(), E]) -> Reader[T, E]

export fn next[T, E](reader: &Reader[T, E]) -> Option[Messaging.Outcome[T, E]]

export fn rest[T, E](reader: &Reader[T, E]) -> Messaging.Outcome[Array[T], E]

export fn close[T, E](reader: &Reader[T, E]) -> ()
```

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Constants/Messages]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Stream]], [[src/PuduLangMediator/Utils/Threads]].
- **Consumed by:** [[src/PuduLangMediator/Mediator]].

## Algorithm

1. `open` starts the producer on a thread with a child token and a sink that sends into a channel of `capacity` items (at least one); the producer waits while the channel is full.
2. The producer runs contained, so a crash ends the stream with `Crashed` instead of leaving the consumer waiting.
3. `next` answers items, then the failure once if there is one, then `None`.
4. `close` cancels the producer's token, closes the channel (waking a producer waiting to send), and joins the thread.

## Negative Logic (Prohibited Paths)

- No producer thread outlives `close`.
- A finished or closed reader never waits.

## Edge Cases

- Cancelling the caller's context stops the producer through its child token.

## Depth

DEPTH 0.8 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why a bounded channel?
  **A:** A fast producer is slowed to its consumer's speed instead of filling memory; the bound is the back-pressure. _Rejected:_ an unbounded queue (postpones the problem until memory runs out).

## Referenced by

[[CHANGELOG]] · [[domain/Stream]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Constants/Messages]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Stream]] · [[src/PuduLangMediator/Utils/Threads]] · [[src/PuduLangMediator/_MOC]] · [[subsystems/Streams]]
