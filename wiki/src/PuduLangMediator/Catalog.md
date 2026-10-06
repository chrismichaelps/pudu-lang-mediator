---
type: module
path: "@root/src/PuduLangMediator/Catalog.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, backbone]
aliases: [PuduLangMediator.Catalog]
---

# PuduLangMediator.Catalog

## Purpose

Every registration gathered before a build: one entry per kind holding its packed route and counts, the open components and marks every kind is built with, and the problems found.

## Interface

### Signatures

```pudu
export type Scope = ForUnhandled | ForAll

export type Assembly = {
  marks: Array[Order.Mark],
  behaviors: Array[dynamic Open.Behavior],
  preProcessors: Array[dynamic Open.PreProcessor],
  postProcessors: Array[dynamic Open.PostProcessor],
  exceptionActions: Array[dynamic Open.ExceptionAction],
  listeners: Array[Open.Listener],
  streamBehaviors: Array[dynamic Open.StreamBehavior],
  publisher: dynamic Publishing.Strategy,
  actionScope: Scope
}

export type Dispatch
  = Answering(fn(Message.Envelope, Context.Context) -> Message.Answer)
  | Streaming(fn(Message.Envelope, Context.Context, fn(Erasure.Packed) -> Bool) -> Message.Answer)

export type Compiled = { info: Message.Info, invoke: Erasure.Packed, dispatch: Dispatch }

export type Entry = {
  info: Message.Info,
  route: Erasure.Packed,
  handlers: Int,
  components: Int,
  collisions: Int,
  finish: fn(Erasure.Packed, Assembly) -> Compiled
}

export type Catalog = {
  requests: HashMap.HashMap[Str, Entry],
  notifications: HashMap.HashMap[Str, Entry],
  streams: HashMap.HashMap[Str, Entry],
  assembly: Assembly
}

export fn empty(publisher: dynamic Publishing.Strategy, actionScope: Scope) -> Catalog

export fn touch[R](
  table: &HashMap.HashMap[Str, Entry],
  info: &Message.Info,
  routes: &Erasure.Slot[R],
  blank: R,
  change: fn(R) -> R,
  handlers: Int,
  components: Int,
  finish: fn(Erasure.Packed, Assembly) -> Compiled
) -> HashMap.HashMap[Str, Entry]

export fn marked(catalog: Catalog, layer: Order.Layer, name: Str) -> Catalog

export fn problems(catalog: &Catalog) -> Array[Str]

export fn compile(table: &HashMap.HashMap[Str, Entry], assembly: &Assembly) -> HashMap.HashMap[Str, Compiled]
```

### Linkage

- **Requires:** [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Domain/Census]], [[src/PuduLangMediator/Domain/Order]], [[src/PuduLangMediator/Message]], [[src/PuduLangMediator/Open]], [[src/PuduLangMediator/Publishing]], [[src/PuduLangMediator/Utils/Erasure]].
- **Consumed by:** [[src/PuduLangMediator/Compose]], [[src/PuduLangMediator/Mediator]], [[src/PuduLangMediator/Registration]].

## Algorithm

1. `touch` applies a change to a kind's route: the first registration starts from a blank route; a later one unpacks through the kind's route slot; a route that does not unpack belongs to another kind under the same name and counts as a collision.
2. `marked` appends a kind's ordered registration to the marks.
3. `problems` takes a census of every entry; `compile` runs each entry's `finish` with the assembly.

## Negative Logic (Prohibited Paths)

- A collision never changes the first kind's route.

## Edge Cases

- Tables keep insertion order, so kinds are listed in registration order.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why does each entry carry its own `finish`?
  **A:** The catalog holds entries of every type erased; the registration that created an entry knew its types and leaves the typed composition behind as a closure. _Rejected:_ a type switch at build time (impossible without runtime types).

## Referenced by

[[architecture/_MOC]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Domain/Census]] · [[src/PuduLangMediator/Domain/Order]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Message]] · [[src/PuduLangMediator/Open]] · [[src/PuduLangMediator/Publishing]] · [[src/PuduLangMediator/Registration]] · [[src/PuduLangMediator/Utils/Erasure]] · [[src/PuduLangMediator/_MOC]]
