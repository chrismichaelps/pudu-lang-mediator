---
type: module
path: "@root/src/PuduLangMediator/Mediator.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.8
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangMediator.Mediator]
---

# PuduLangMediator.Mediator

## Purpose

Routes requests, notifications, and streams to their composed pipelines: builds a mediator from registrations, and sends, publishes, streams, and dispatches envelopes through it.

## Interface

### Signatures

```pudu
export type Options = { publisher: dynamic Publishing.Strategy, actionScope: Catalog.Scope }

export type Mediator = {
  requests: HashMap.HashMap[Str, Catalog.Compiled],
  notifications: HashMap.HashMap[Str, Catalog.Compiled],
  streams: HashMap.HashMap[Str, Catalog.Compiled],
  assembly: Catalog.Assembly
}

export type Invalid = { problems: Array[Str] }

export fn defaults() -> Options

export fn build(registrations: Array[Registration.Registration]) -> Result[Mediator, Invalid]

export fn buildWith(options: &Options, registrations: Array[Registration.Registration]) -> Result[Mediator, Invalid]

export fn explain(invalid: &Invalid) -> Str

export fn kinds(mediator: &Mediator) -> Array[Message.Info]

export fn resolve[Q, R, E](mediator: &Mediator, subject: &Request.Kind[Q, R, E]) -> Result[Request.Handler[Q, R, E], Messaging.Failure[E]]

export fn send[Q, R, E](mediator: &Mediator, subject: &Request.Kind[Q, R, E], request: Q) -> Messaging.Outcome[R, E]

export fn sendWith[Q, R, E](mediator: &Mediator, subject: &Request.Kind[Q, R, E], request: Q, context: Context.Context) -> Messaging.Outcome[R, E]

export fn dispatch(mediator: &Mediator, envelope: Message.Envelope) -> Message.Answer

export fn dispatchWith(mediator: &Mediator, envelope: Message.Envelope, context: Context.Context) -> Message.Answer

export fn publish[N, E](mediator: &Mediator, subject: &Notification.Kind[N, E], notification: N) -> Messaging.Outcome[(), E]

export fn publishWith[N, E](mediator: &Mediator, subject: &Notification.Kind[N, E], notification: N, context: Context.Context) -> Messaging.Outcome[(), E]

export fn broadcast(mediator: &Mediator, envelope: Message.Envelope) -> Message.Answer

export fn broadcastWith(mediator: &Mediator, envelope: Message.Envelope, context: Context.Context) -> Message.Answer

export fn stream[Q, T, E](mediator: &Mediator, subject: &Stream.Kind[Q, T, E], request: Q, sink: Stream.Sink[T]) -> Messaging.Outcome[(), E]

export fn streamWith[Q, T, E](mediator: &Mediator, subject: &Stream.Kind[Q, T, E], request: Q, context: Context.Context, sink: Stream.Sink[T]) -> Messaging.Outcome[(), E]

export fn collect[Q, T, E](mediator: &Mediator, subject: &Stream.Kind[Q, T, E], request: Q) -> Messaging.Outcome[Array[T], E]

export fn reader[Q, T, E](mediator: &Mediator, subject: &Stream.Kind[Q, T, E], request: Q, context: &Context.Context, capacity: Int) -> Reader.Reader[T, E]

export fn streamEnvelope(mediator: &Mediator, envelope: Message.Envelope, context: Context.Context, sink: fn(Erasure.Packed) -> Bool) -> Message.Answer
```

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Catalog]], [[src/PuduLangMediator/Compose]], [[src/PuduLangMediator/Constants/Messages]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Message]], [[src/PuduLangMediator/Notification]], [[src/PuduLangMediator/Publishing]], [[src/PuduLangMediator/Reader]], [[src/PuduLangMediator/Registration]], [[src/PuduLangMediator/Request]], [[src/PuduLangMediator/Stream]], [[src/PuduLangMediator/Utils/Erasure]], [[src/PuduLangMediator/Utils/Shared]], [[src/PuduLangMediator/Utils/Template]].
- **Consumed by:** [[src/PuduLangMediator/Publisher]], [[src/PuduLangMediator/Sender]].

## Algorithm

1. `buildWith` applies the registrations in order, refuses the build with every problem, and otherwise composes every kind once.
2. A typed send looks up the kind's name and unpacks the composed handler through the kind's own slot: absent is `Unhandled`, another kind's is `Mismatched`.
3. `resolve` answers the composed handler itself, so a hot path looks it up once.
4. A publication of a kind with no handlers runs the open handlers only; a broadcast of an unknown kind is unheard.
5. `reader` runs a stream on its own thread behind a bounded channel.

## Negative Logic (Prohibited Paths)

- Nothing is resolved by runtime type; every route is the kind value the caller holds.
- A send never runs a handler registered for another kind of the same name.

## Edge Cases

- `kinds` lists requests, then notifications, then streams, each in registration order.

## Depth

DEPTH 0.8 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why refuse a build instead of failing at the first send?
  **A:** Registration mistakes are programming errors found once at startup; a running service should never discover a missing handler from traffic. _Rejected:_ lazy resolution on first send.
- **Q:** How does a handler publish through the mediator it belongs to?
  **A:** It captures a `Shared[Option[Mediator]]` set right after the build; the scenario suite does exactly this. _Rejected:_ a mediator inside every context (a cycle between the context and the mediator).

## Referenced by

[[CHANGELOG]] · [[architecture/_MOC]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Catalog]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Constants/Messages]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Message]] · [[src/PuduLangMediator/Notification]] · [[src/PuduLangMediator/Publisher]] · [[src/PuduLangMediator/Publishing]] · [[src/PuduLangMediator/Reader]] · [[src/PuduLangMediator/Registration]] · [[src/PuduLangMediator/Request]] · [[src/PuduLangMediator/Sender]] · [[src/PuduLangMediator/Stream]] · [[src/PuduLangMediator/Utils/Erasure]] · [[src/PuduLangMediator/Utils/Shared]] · [[src/PuduLangMediator/Utils/Template]] · [[src/PuduLangMediator/_MOC]] · [[subsystems/Notifications]] · [[subsystems/Requests]]
