---
type: module
path: "@root/src/PuduLangMediator/Request.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangMediator.Request]
---

# PuduLangMediator.Request

## Purpose

Requests answered by exactly one handler: the typed `Kind[Q, R, E]`, the handler, behavior, processor, and exception component types, the `Route` that gathers them, and envelopes and answers for untyped dispatch.

## Interface

### Signatures

```pudu
export type Handler[Q, R, E] = fn(Q, Context.Context) -> Messaging.Outcome[R, E]

export type Next[R, E] = fn(Context.Context) -> Messaging.Outcome[R, E]

export type Behavior[Q, R, E] = fn(Q, Context.Context, Next[R, E]) -> Messaging.Outcome[R, E]

export type PreProcessor[Q, E] = fn(Q, Context.Context) -> Messaging.Outcome[(), E]

export type PostProcessor[Q, R, E] = fn(Q, R, Context.Context) -> Messaging.Outcome[(), E]

export type ExceptionHandler[Q, R, E] = fn(Q, Messaging.Failure[E], Context.Context) -> Option[R]

export type ExceptionAction[Q, E] = fn(Q, Messaging.Failure[E], Context.Context) -> ()

export type Route[Q, R, E] = {
  handlers: Array[Handler[Q, R, E]],
  behaviors: Array[Behavior[Q, R, E]],
  preProcessors: Array[PreProcessor[Q, E]],
  postProcessors: Array[PostProcessor[Q, R, E]],
  exceptionHandlers: Array[ExceptionHandler[Q, R, E]],
  exceptionActions: Array[ExceptionAction[Q, E]]
}

export type Kind[Q, R, E] = {
  info: Message.Info,
  routes: Erasure.Slot[Route[Q, R, E]],
  invokers: Erasure.Slot[Handler[Q, R, E]],
  requests: Erasure.Slot[Q],
  outcomes: Erasure.Slot[Messaging.Outcome[R, E]]
}

export fn kind[Q, R, E](name: Str) -> Kind[Q, R, E]

export fn command[Q, E](name: Str) -> Kind[Q, (), E]

export fn tagged[Q, R, E](subject: &Kind[Q, R, E], tags: Array[Str]) -> Kind[Q, R, E]

export fn route[Q, R, E]() -> Route[Q, R, E]

export fn envelope[Q, R, E](subject: &Kind[Q, R, E], request: Q) -> Message.Envelope

export fn answerOf[Q, R, E](subject: &Kind[Q, R, E], answer: &Message.Answer) -> Option[Messaging.Outcome[R, E]]
```

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Message]], [[src/PuduLangMediator/Utils/Erasure]].
- **Consumed by:** [[src/PuduLangMediator/Behaviors/Logging]], [[src/PuduLangMediator/Behaviors/Resilient]], [[src/PuduLangMediator/Behaviors/Validation]], [[src/PuduLangMediator/Compose]], [[src/PuduLangMediator/Mediator]], [[src/PuduLangMediator/Open]], [[src/PuduLangMediator/Registration]], [[src/PuduLangMediator/Sender]].

## Algorithm

1. `kind` makes a query (answers `R`) and `command` a request answering `()`; each holds fresh slots for its route, composed handler, requests, and outcomes.
2. `tagged` adds tags open components may read; the slots are shared, so it is the same kind.
3. `envelope` and `answerOf` cross the untyped boundary through the kind's own slots.

## Negative Logic (Prohibited Paths)

- A kind made twice under one name is two kinds: neither reads the other's envelopes or answers.

## Edge Cases

- `Next` takes a context, so a behavior may hand the rest of the pipeline another token.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why typed kind values instead of resolving a handler from the request's type?
  **A:** Pudu resolves nothing by runtime type; a kind value is the typed identity both the registration and the send site hold, and its slots make the round trip type-safe. _Rejected:_ a trait implemented by each request type (handlers could not carry dependencies or be registered per mediator).
- **Q:** Why is the error type part of the kind?
  **A:** A handler's error is domain data a caller matches on; fixing one error type for every kind would force text errors on every handler. _Rejected:_ one package-wide error record.

## Referenced by

[[CHANGELOG]] · [[domain/Kind]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Behaviors/Logging]] · [[src/PuduLangMediator/Behaviors/Resilient]] · [[src/PuduLangMediator/Behaviors/Validation]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Message]] · [[src/PuduLangMediator/Open]] · [[src/PuduLangMediator/Registration]] · [[src/PuduLangMediator/Sender]] · [[src/PuduLangMediator/Utils/Erasure]] · [[src/PuduLangMediator/_MOC]] · [[subsystems/Requests]]
