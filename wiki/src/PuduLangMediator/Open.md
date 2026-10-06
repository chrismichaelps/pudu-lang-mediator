---
type: module
path: "@root/src/PuduLangMediator/Open.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MEDIUM
tags: [module, seam]
aliases: [PuduLangMediator.Open]
---

# PuduLangMediator.Open

## Purpose

Components that apply to every kind: open behaviors, pre-processors, post-processors, exception actions, notification handlers, and stream behaviors, each a trait whose one member is generic over the message types.

## Interface

### Signatures

```pudu
export trait Behavior {
  fn handleRequest[Q, R, E](self: &Self, request: &Q, info: &Message.Info, context: Context.Context, next: Request.Next[R, E]) -> Messaging.Outcome[R, E]
}

export trait PreProcessor {
  fn beforeRequest[Q, E](self: &Self, request: &Q, info: &Message.Info, context: Context.Context) -> Messaging.Outcome[(), E]
}

export trait PostProcessor {
  fn afterRequest[Q, R, E](self: &Self, request: &Q, response: &R, info: &Message.Info, context: Context.Context) -> Messaging.Outcome[(), E]
}

export trait ExceptionAction {
  fn onFailure[Q, E](self: &Self, request: &Q, failure: &Messaging.Failure[E], info: &Message.Info, context: Context.Context) -> ()
}

export trait NotificationHandler {
  fn handleNotification[N, E](self: &Self, notification: &N, info: &Message.Info, context: Context.Context) -> Messaging.Outcome[(), E]
}

export trait StreamBehavior {
  fn handleStream[Q, T, E](self: &Self, request: &Q, info: &Message.Info, context: Context.Context, sink: Stream.Sink[T], next: Stream.Next[T, E]) -> Messaging.Outcome[(), E]
}

export type Listener = { name: Str, handler: dynamic NotificationHandler }
```

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Message]], [[src/PuduLangMediator/Request]], [[src/PuduLangMediator/Stream]].
- **Consumed by:** [[src/PuduLangMediator/Behaviors/Logging]], [[src/PuduLangMediator/Catalog]], [[src/PuduLangMediator/Compose]], [[src/PuduLangMediator/Registration]].

## Algorithm

1. A program implements a trait for its own type and registers a value of it; the mediator holds it as `dynamic` and calls it for every kind in its layer.
2. Each trait's member has its own name, so one type may implement every trait.

## Negative Logic (Prohibited Paths)

- An open component cannot build a value of `R` or an error `E`; it runs `next`, answers what `next` answered, or refuses.

## Edge Cases

- An open component reads the kind's `Info` and tags to decide whether to act.

## Depth

DEPTH 0.6 (MEDIUM). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why traits with generic members instead of closures?
  **A:** A closure value has one type; a component that applies to every kind must be generic in the request, response, and error types, which only a trait member can be. _Rejected:_ closures over an erased view (every component would unpack and repack values).
- **Q:** Why distinct member names across the traits?
  **A:** One type implementing two traits with the same member name is ambiguous where it is widened; distinct names let one recorder log requests, streams, and notifications. _Rejected:_ one `handle` member per trait.

## Referenced by

[[CHANGELOG]] · [[seams/Open]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Behaviors/Logging]] · [[src/PuduLangMediator/Catalog]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Message]] · [[src/PuduLangMediator/Registration]] · [[src/PuduLangMediator/Request]] · [[src/PuduLangMediator/Stream]] · [[src/PuduLangMediator/_MOC]]
