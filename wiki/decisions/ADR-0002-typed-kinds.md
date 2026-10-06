---
type: adr
status: Accepted
tags: [adr]
---

# ADR-0002 — Messages are routed by typed kind values

## Context

A mediator holds handlers of many request, response, and error types and must hand each message
to the right one with its types intact. Pudu has no runtime type information and no casts.

## Decision

Each message has a kind value made once ([[domain/Kind]]). A kind carries slots
([[src/PuduLangMediator/Utils/Erasure]]); everything the mediator stores about a kind is packed
through them and read back through the same kind at the send site.

## Consequences

- A send is a hash lookup and an unpack; a mistaken twin kind answers `Mismatched` instead of
  running the wrong handler.
- Kinds are runtime values, so a program makes them at startup and shares them, the way it shares
  the mediator.

## Rejected

- One closed sum of every message: every new message would edit a central type.
- Text-serialized messages: loses types and pays for encoding on every send.

## Referenced by

[[CHANGELOG]] · [[decisions/_MOC]] · [[domain/Kind]] · [[handoffs/2026-10-06-initial-package]]
