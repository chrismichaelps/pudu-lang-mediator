---
type: adr
status: Accepted
tags: [adr]
---

# ADR-0001 — Every handler and component answers an outcome

## Context

Behaviors, processors, and exception components decide from what the inner layers answered.
Pudu has no exceptions to catch, and a panic ends the program.

## Decision

Every handler, behavior, processor, and publication answers `Outcome[T, E]`, a
`Result[T, Failure[E]]` ([[src/PuduLangMediator]]). Exception handlers and actions receive the
failure as a value.

## Consequences

- Every refusal the mediator makes is a value the caller matches on.
- A thread that stops in a parallel publication or a stream reader is reported as `Crashed`.

## Rejected

- Panicking on a missing handler: one unregistered message would stop a service.

## Referenced by

[[CHANGELOG]] · [[decisions/_MOC]] · [[domain/Outcome]] · [[handoffs/2026-10-06-initial-package]]
