---
type: seam
capacity: BACKBONE
tags: [seam, backbone]
---

# Open components (seam)

## Classification

Extension boundary for components that apply to every kind: the traits of
[[src/PuduLangMediator/Open]], held as `dynamic` values and called with each kind's types.

## Adapters

- **Program types** — logging, timing, auditing, authorization, caching keys.
- **Integrations** — the [[src/PuduLangMediator/Behaviors/Logging]] recorder implements three of
  them at once.

## Health

An open component can run the rest of the pipeline, answer what it answered, or refuse; it cannot
invent a response or an error of the kind's types.

## Referenced by

[[architecture/LANGUAGE]] · [[seams/_MOC]]
