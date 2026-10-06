---
type: adr
status: Accepted
tags: [adr]
---

# ADR-0004 — Registrations are explicit and validated at build time

## Context

A program cannot be scanned for handlers at run time, and a handler's dependencies are values it
captures.

## Decision

A mediator is built from an array of registrations ([[src/PuduLangMediator/Registration]]). The
build reports every problem at once — a kind without a name, two kinds under one name, a request
or stream kind without exactly one handler — and composes nothing until there are none
([[src/PuduLangMediator/Domain/Census]]).

## Consequences

- A handler's lifetime is the lifetime of what it captures: build the mediator once for shared
  handlers, or capture factories for per-message state.
- Registration mistakes surface once at startup.

## Rejected

- Lazy resolution on first send: the first message of a kind would discover a missing handler.

## Referenced by

[[CHANGELOG]] · [[decisions/_MOC]] · [[handoffs/2026-10-06-initial-package]]
