---
type: module
path: "@root/src/PuduLangMediator/Registration.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MEDIUM
tags: [module, backbone]
aliases: [PuduLangMediator.Registration]
---

# PuduLangMediator.Registration

## Purpose

The handlers and components a mediator is built from: one constructor per kind of registration, each a change to the catalog, and `group` to keep related registrations together.

## Interface

### Signatures

```pudu
export type Registration = { apply: fn(Catalog.Catalog) -> Catalog.Catalog }

export fn handler[Q, R, E](subject: &Request.Kind[Q, R, E], handle: Request.Handler[Q, R, E]) -> Registration

export fn behavior[Q, R, E](subject: &Request.Kind[Q, R, E], wrap: Request.Behavior[Q, R, E]) -> Registration

export fn preProcessor[Q, R, E](subject: &Request.Kind[Q, R, E], process: Request.PreProcessor[Q, E]) -> Registration

export fn postProcessor[Q, R, E](subject: &Request.Kind[Q, R, E], process: Request.PostProcessor[Q, R, E]) -> Registration

export fn exceptionHandler[Q, R, E](subject: &Request.Kind[Q, R, E], recover: Request.ExceptionHandler[Q, R, E]) -> Registration

export fn exceptionAction[Q, R, E](subject: &Request.Kind[Q, R, E], act: Request.ExceptionAction[Q, E]) -> Registration

export fn notificationHandler[N, E](subject: &Notification.Kind[N, E], name: Str, handle: Notification.Handler[N, E]) -> Registration

export fn streamHandler[Q, T, E](subject: &Stream.Kind[Q, T, E], handle: Stream.Handler[Q, T, E]) -> Registration

export fn streamBehavior[Q, T, E](subject: &Stream.Kind[Q, T, E], wrap: Stream.Behavior[Q, T, E]) -> Registration

export fn openBehavior(open: dynamic Open.Behavior) -> Registration

export fn openPreProcessor(open: dynamic Open.PreProcessor) -> Registration

export fn openPostProcessor(open: dynamic Open.PostProcessor) -> Registration

export fn openExceptionAction(open: dynamic Open.ExceptionAction) -> Registration

export fn openNotificationHandler(name: Str, open: dynamic Open.NotificationHandler) -> Registration

export fn openStreamBehavior(open: dynamic Open.StreamBehavior) -> Registration

export fn group(registrations: Array[Registration]) -> Registration
```

### Linkage

- **Requires:** [[src/PuduLangMediator/Catalog]], [[src/PuduLangMediator/Compose]], [[src/PuduLangMediator/Domain/Order]], [[src/PuduLangMediator/Notification]], [[src/PuduLangMediator/Open]], [[src/PuduLangMediator/Request]], [[src/PuduLangMediator/Stream]].
- **Consumed by:** [[src/PuduLangMediator/Behaviors/Resilient]], [[src/PuduLangMediator/Behaviors/Validation]], [[src/PuduLangMediator/Mediator]].

## Algorithm

1. Typed registrations change their kind's route through `Catalog.touch` and mark their layer when order among other kinds' components matters.
2. Open registrations append to the assembly and mark their layer with their index.

## Negative Logic (Prohibited Paths)

- A registration changes nothing until a build applies it.

## Edge Cases

- Exception handlers are ordered within their kind only; they have no open form.

## Depth

DEPTH 0.6 (MEDIUM). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why an array of registrations instead of a builder chain?
  **A:** An array reads flat, composes with `group`, and is validated as a whole by the build, which reports every problem at once. _Rejected:_ a fluent builder (methods need a trait per builder and nest poorly).

## Referenced by

[[CHANGELOG]] · [[decisions/ADR-0004-explicit-registration]] · [[src/PuduLangMediator/Behaviors/Resilient]] · [[src/PuduLangMediator/Behaviors/Validation]] · [[src/PuduLangMediator/Catalog]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Domain/Order]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Notification]] · [[src/PuduLangMediator/Open]] · [[src/PuduLangMediator/Request]] · [[src/PuduLangMediator/Stream]] · [[src/PuduLangMediator/_MOC]] · [[subsystems/Requests]]
