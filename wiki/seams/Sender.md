---
type: seam
capacity: LEAF
tags: [seam]
---

# Sender and publisher (seam)

## Classification

Dependency boundary for components that only send or only publish:
[[src/PuduLangMediator/Sender]] and [[src/PuduLangMediator/Publisher]], both implemented by the
mediator.

## Adapters

- **Mediator** — the built mediator itself.
- **Doubles** — a refusing or recording implementation in a suite.

## Health

A component's dependency names exactly what it does.

## Referenced by

[[seams/_MOC]]
