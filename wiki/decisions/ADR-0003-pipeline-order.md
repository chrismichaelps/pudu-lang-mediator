---
type: adr
status: Accepted
tags: [adr]
---

# ADR-0003 — One documented order for every layer

## Context

Behaviors, processors, and exception components can be open or per kind and registered in any
order. Their order changes what each observes.

## Decision

Outermost first: exception actions and exception handlers (actions outside for `ForUnhandled`,
the default, inside for `ForAll`), pre-processors, post-processors, then behaviors open and per
kind in registration order, then the handler ([[domain/Pipeline]],
[[src/PuduLangMediator/Compose]]). Within each layer, open and per-kind components keep their
registration order ([[src/PuduLangMediator/Domain/Order]]).

## Consequences

- Exception components see failures from every layer, including processors.
- Post-processors run only on a value and before any behavior sees it on the way out.

## Rejected

- Processors as ordinary behaviors: their place would depend on where they were registered.

## Referenced by

[[CHANGELOG]] · [[decisions/_MOC]] · [[domain/Pipeline]] · [[handoffs/2026-10-06-initial-package]] · [[src/PuduLangMediator/Compose]]
