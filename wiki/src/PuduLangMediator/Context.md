---
type: module
path: "@root/src/PuduLangMediator/Context.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangMediator.Context]
---

# PuduLangMediator.Context

## Purpose

What one message carries through its pipeline: the cancellation token every handler and behavior observes, and typed items shared between them.

## Interface

### Signatures

```pudu
export type Context = { token: Cancel.Token, items: Shared.Shared[HashMap.HashMap[Str, Erasure.Packed]] }

export type Key[T] = { name: Str, slot: Erasure.Slot[T] }

export fn create() -> Context

export fn cancellable(token: Cancel.Token) -> Context

export fn withToken(source: &Context, token: Cancel.Token) -> Context

export fn expiring(source: &Context, millis: Int) -> Context

export fn key[T](name: Str) -> Key[T]

export fn set[T](target: &Context, name: &Key[T], value: T) -> ()

export fn get[T](source: &Context, name: &Key[T]) -> Option[T]

export fn getOr[T](source: &Context, name: &Key[T], fallback: T) -> T

export fn has[T](source: &Context, name: &Key[T]) -> Bool

export fn remove[T](target: &Context, name: &Key[T]) -> ()

export fn names(source: &Context) -> Array[Str]

export fn cancel(target: &Context, why: Str) -> ()

export fn stopped(source: &Context) -> Bool

export fn check[E](source: &Context) -> Result[(), Messaging.Failure[E]]

export fn pause[E](source: &Context, millis: Int) -> Result[(), Messaging.Failure[E]]
```

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Utils/Erasure]], [[src/PuduLangMediator/Utils/Shared]].
- **Consumed by:** [[src/PuduLangMediator/Behaviors/Logging]], [[src/PuduLangMediator/Behaviors/Resilient]], [[src/PuduLangMediator/Behaviors/Validation]], [[src/PuduLangMediator/Catalog]], [[src/PuduLangMediator/Compose]], [[src/PuduLangMediator/Mediator]], [[src/PuduLangMediator/Notification]], [[src/PuduLangMediator/Open]], [[src/PuduLangMediator/Publisher]], [[src/PuduLangMediator/Publishing]], [[src/PuduLangMediator/Reader]], [[src/PuduLangMediator/Request]], [[src/PuduLangMediator/Sender]], [[src/PuduLangMediator/Stream]].

## Algorithm

1. Items live in a shared hash map of packed values under names; a `Key[T]` holds the name and the slot its items are packed through.
2. `withToken` and `expiring` answer the same items under another token; `expiring` uses a child token so the parent's cancellation still reaches it.
3. `check` and `pause` answer `Cancelled` with the token's reason once it fires, so a handler stops with `?`.

## Negative Logic (Prohibited Paths)

- An item never reads back as a type other than the key it was stored through.

## Edge Cases

- Two keys made separately under one name do not read each other's items; `has` still reports the name present.
- The first cancellation reason is kept.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why typed keys backed by slots instead of text-encoded properties?
  **A:** Handlers share records, not just strings; a slot keeps the item's real type without an encoder per type. _Rejected:_ encode and decode functions per key (boilerplate and lossy).
- **Q:** Why does a derived context share items?
  **A:** A behavior that hands the rest of the pipeline another token (a timeout, a deadline) still expects the handler to see the items set before it. _Rejected:_ copying items on every derivation (writes made inside would be lost to the caller).

## Referenced by

[[CHANGELOG]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Behaviors/Logging]] · [[src/PuduLangMediator/Behaviors/Resilient]] · [[src/PuduLangMediator/Behaviors/Validation]] · [[src/PuduLangMediator/Catalog]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Notification]] · [[src/PuduLangMediator/Open]] · [[src/PuduLangMediator/Publisher]] · [[src/PuduLangMediator/Publishing]] · [[src/PuduLangMediator/Reader]] · [[src/PuduLangMediator/Request]] · [[src/PuduLangMediator/Sender]] · [[src/PuduLangMediator/Stream]] · [[src/PuduLangMediator/Utils/Erasure]] · [[src/PuduLangMediator/Utils/Shared]] · [[src/PuduLangMediator/_MOC]]
