---
type: adr
status: Accepted
tags: [adr]
---

# ADR-0005 — Other packages are used only under `Behaviors/`

## Context

Validation, structured logging, and resilience are the behaviors most programs add first, and
three packages already provide them.

## Decision

`Behaviors/Validation`, `Behaviors/Logging`, and `Behaviors/Resilient` use
`pudu-lang-validator`, `pudu-lang-log`, and `pudu-lang-resilience`
([[subsystems/Integrations]]). No module outside `Behaviors/` imports another package.

## Consequences

- The core stays readable on its own and could ship without them.
- The integrations follow the same registration surface as any program behavior.

## Rejected

- Hand-written validation, logging, and retry inside the package: duplicates maintained packages.

## Referenced by

[[CHANGELOG]] · [[architecture/_MOC]] · [[decisions/_MOC]] · [[handoffs/2026-10-06-initial-package]] · [[subsystems/Integrations]]
