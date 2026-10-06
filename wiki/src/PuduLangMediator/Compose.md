---
type: module
path: "@root/src/PuduLangMediator/Compose.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.8
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangMediator.Compose]
---

# PuduLangMediator.Compose

## Purpose

One kind's pipeline from its route and the open components, composed once at build time, plus the finishers that compose each kind and answer its envelopes.

## Interface

### Signatures

```pudu
export fn requestHandler[Q, R, E](info: &Message.Info, route: &Request.Route[Q, R, E], assembly: &Catalog.Assembly) -> Request.Handler[Q, R, E]

export fn publication[N, E](info: &Message.Info, route: &Notification.Route[N, E], assembly: &Catalog.Assembly) -> Notification.Handler[N, E]

export fn streamHandler[Q, T, E](info: &Message.Info, route: &Stream.Route[Q, T, E], assembly: &Catalog.Assembly) -> Stream.Handler[Q, T, E]

export fn requestFinish[Q, R, E](subject: Request.Kind[Q, R, E]) -> fn(Erasure.Packed, Catalog.Assembly) -> Catalog.Compiled

export fn notificationFinish[N, E](subject: Notification.Kind[N, E]) -> fn(Erasure.Packed, Catalog.Assembly) -> Catalog.Compiled

export fn streamFinish[Q, T, E](subject: Stream.Kind[Q, T, E]) -> fn(Erasure.Packed, Catalog.Assembly) -> Catalog.Compiled
```

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Catalog]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Domain/Order]], [[src/PuduLangMediator/Message]], [[src/PuduLangMediator/Notification]], [[src/PuduLangMediator/Open]], [[src/PuduLangMediator/Publishing]], [[src/PuduLangMediator/Request]], [[src/PuduLangMediator/Stream]], [[src/PuduLangMediator/Utils/Erasure]].
- **Consumed by:** [[src/PuduLangMediator/Mediator]], [[src/PuduLangMediator/Registration]].

## Algorithm

1. A request handler is wrapped, outermost first, in: exception actions and exception handlers (actions outside for `ForUnhandled`, inside for `ForAll`), the pre-processors, the post-processors, and the behaviors in registration order.
2. Pre-processors run in registration order before the rest; post-processors run in registration order after a value; exception handlers are tried in order and the first value wins; actions all observe a failure that keeps propagating.
3. A publication collects the kind's own and the open notification handlers in registration order and hands them to the strategy as executors.
4. A stream handler is wrapped in its stream behaviors, open and its own, in registration order.

## Negative Logic (Prohibited Paths)

- A layer with no components is left out entirely, so an unused feature costs nothing per message.
- Composition happens once per kind at build time; a message pays only for the closures it passes through.

## Edge Cases

- A route without a handler composes to `Unhandled`; a build refuses such a request or stream kind first.

## Depth

DEPTH 0.8 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why exception layers outermost and processors before behaviors?
  **A:** Exception handling must see failures from every inner layer, processors bracket the handler for every behavior to observe, and behaviors keep the order they were registered in; this is the documented order of [[decisions/ADR-0003-pipeline-order]]. _Rejected:_ processors as ordinary behaviors in registration order (their place would depend on where they were registered).
- **Q:** Why compose at build time?
  **A:** Every message of a kind runs the same layers; composing once leaves a lookup and a call on the hot path. _Rejected:_ composing per message.

## Referenced by

[[CHANGELOG]] · [[architecture/_MOC]] · [[decisions/ADR-0003-pipeline-order]] · [[domain/Pipeline]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Catalog]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Domain/Order]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Message]] · [[src/PuduLangMediator/Notification]] · [[src/PuduLangMediator/Open]] · [[src/PuduLangMediator/Publishing]] · [[src/PuduLangMediator/Registration]] · [[src/PuduLangMediator/Request]] · [[src/PuduLangMediator/Stream]] · [[src/PuduLangMediator/Utils/Erasure]] · [[src/PuduLangMediator/_MOC]] · [[subsystems/Requests]]
