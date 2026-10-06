---
type: module
path: "@root/src/PuduLangMediator/Domain/Order.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, domain, pure]
aliases: [PuduLangMediator.Domain.Order]
---

# PuduLangMediator.Domain.Order

## Purpose

Where each component runs in a pipeline: registrations are marked in order, open or for one kind, and `picks` answers the components of one layer that apply to one kind, in registration order.

## Interface

### Signatures

```pudu
export type Layer = Behaviors | PreProcessors | PostProcessors | ExceptionActions | NotificationHandlers | StreamBehaviors

export type Mark = Open(Layer, Int) | Closed(Layer, Str)

export type Pick = Shared(Int) | Own(Int)

export fn picks(marks: &Array[Mark], layer: Layer, name: Str) -> Array[Pick]
```

### Linkage

- **Requires:** standard library only.
- **Consumed by:** [[src/PuduLangMediator/Catalog]], [[src/PuduLangMediator/Compose]], [[src/PuduLangMediator/Registration]].

## Algorithm

1. Every ordered registration appends one `Mark`: `Open(layer, index)` for an open component, `Closed(layer, name)` for a kind's own.
2. `picks` walks the marks once; an open mark of the layer answers `Shared(index)`, and a closed mark of the layer and kind answers `Own(n)`, counting the kind's own components from zero.

## Negative Logic (Prohibited Paths)

- Performs no effects and imports nothing; it is the mutation-tested core of ordering.

## Edge Cases

- Another kind's own components are skipped without advancing this kind's count.
- A layer with no marks picks nothing, so composition leaves that layer out.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why marks instead of sorting by a sequence number?
  **A:** Registration order is already the mark order; walking it once needs no numbers to keep consistent across open and closed lists. _Rejected:_ a global counter stored on every component (two sources of order).

## Referenced by

[[decisions/ADR-0003-pipeline-order]] · [[src/PuduLangMediator/Catalog]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Domain/_MOC]] · [[src/PuduLangMediator/Registration]]
