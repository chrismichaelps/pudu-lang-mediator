---
type: module
path: "@root/src/PuduLangMediator/Behaviors/Validation.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MEDIUM
tags: [module, integration]
aliases: [PuduLangMediator.Behaviors.Validation]
---

# PuduLangMediator.Behaviors.Validation

## Purpose

Requests and streams refused when their rules fail: a per-kind behavior that runs a `pudu-lang-validator` validator and answers `Invalid` with every violation.

## Interface

### Signatures

```pudu
export fn behavior[Q, R, E](validator: Validator.Validator[Q]) -> Request.Behavior[Q, R, E]

export fn register[Q, R, E](subject: &Request.Kind[Q, R, E], validator: Validator.Validator[Q]) -> Registration.Registration

export fn streamBehavior[Q, T, E](validator: Validator.Validator[Q]) -> Stream.Behavior[Q, T, E]

export fn registerStream[Q, T, E](subject: &Stream.Kind[Q, T, E], validator: Validator.Validator[Q]) -> Registration.Registration

export fn violationsOf(result: &Validated.ValidationResult) -> Array[Messaging.Violation]
```

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Registration]], [[src/PuduLangMediator/Request]], [[src/PuduLangMediator/Stream]]; packages: `PuduLangValidator.Result`, `PuduLangValidator.Validator`.
- **Consumed by:** programs using the package.

## Algorithm

1. The validator runs on the request; a valid request continues, an invalid one answers `Invalid` with each failure's property, message, and code, in rule order.

## Negative Logic (Prohibited Paths)

- An invalid request never reaches its handler or later behaviors.
- No other module of the core imports the validator.

## Edge Cases

- An invalid stream emits nothing.

## Depth

DEPTH 0.6 (MEDIUM). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why per kind rather than open?
  **A:** A validator is typed by the request it checks; an open behavior cannot hold one validator for every type. _Rejected:_ an open behavior with a registry of validators (an erased lookup per message).

## Referenced by

[[src/PuduLangMediator]] · [[src/PuduLangMediator/Behaviors/_MOC]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Registration]] · [[src/PuduLangMediator/Request]] · [[src/PuduLangMediator/Stream]] · [[subsystems/Integrations]]
