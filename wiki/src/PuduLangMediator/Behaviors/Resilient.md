---
type: module
path: "@root/src/PuduLangMediator/Behaviors/Resilient.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.7
depth_status: DEEP
tags: [module, integration]
aliases: [PuduLangMediator.Behaviors.Resilient]
---

# PuduLangMediator.Behaviors.Resilient

## Purpose

Requests run through a `pudu-lang-resilience` pipeline: retries, timeouts, circuit breaking, rate limiting, hedging, and fallback around the rest of a kind's pipeline.

## Interface

### Signatures

```pudu
export fn behavior[Q, R, E](pipeline: Pipeline.Pipeline[R, Messaging.Failure[E]]) -> Request.Behavior[Q, R, E]

export fn register[Q, R, E](subject: &Request.Kind[Q, R, E], pipeline: Pipeline.Pipeline[R, Messaging.Failure[E]]) -> Registration.Registration

export fn translated[E](failure: &Resilience.Failure[Messaging.Failure[E]]) -> Messaging.Failure[E]
```

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Registration]], [[src/PuduLangMediator/Request]]; packages: `PuduLangResilience`, `PuduLangResilience.Context`, `PuduLangResilience.Pipeline`.
- **Consumed by:** programs using the package.

## Algorithm

1. The resilience context observes the message's token; each attempt runs the rest of the pipeline with the attempt's token, so a timeout reaches the handler.
2. A failure of the rest of the pipeline reaches the strategies as `Raised`, except a cancellation, which stays a cancellation so it is never retried.
3. Back across: the pipeline's own failure is unwrapped, a cancellation or crash stays one, and a strategy's rejection is `Refused` with its description.

## Negative Logic (Prohibited Paths)

- A caller's cancellation is never retried.

## Edge Cases

- Exhausted retries answer the last attempt's own failure.

## Depth

DEPTH 0.7 (DEEP). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why translate rejections to `Refused`?
  **A:** The mediator's failure cannot name the resilience package's variants without importing it into the core; the description keeps what happened. _Rejected:_ a variant per rejection in the core vocabulary.

## Referenced by

[[src/PuduLangMediator]] · [[src/PuduLangMediator/Behaviors/_MOC]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Registration]] · [[src/PuduLangMediator/Request]] · [[subsystems/Integrations]]
