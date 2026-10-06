---
type: handoff
from_role: Forensic Guardian
to_role: Architect
status: complete
tags: [handoff, delivery]
---

# Initial package

## Done

- Issue #1 is the ready issue; `feature/1-initial-mediator-package` is branched from `dev`, which
  is branched from the `main` baseline.
- Every module under `src/` has its mirrored page with a resolved Grill Log ([[src/_MOC]]).
- Against the published 0.1.3 compiler: `pudu check`, `pudu fmt --check`, and `pudu lint` are clean
  over `src`, `test`, `tools`, and `examples`; every suite passes; every example answers 0; the
  mutation gate kills 12 of 12 mutants of `Domain/`.
- The vault matches the code, and `test/Package/VaultTest` keeps it so ([[architecture/TESTING]]).

## Decided (do not re-litigate)

- Outcomes, not exceptions ([[decisions/ADR-0001-outcomes-not-exceptions]]); typed kinds
  ([[decisions/ADR-0002-typed-kinds]]); the pipeline order ([[decisions/ADR-0003-pipeline-order]]);
  explicit registration ([[decisions/ADR-0004-explicit-registration]]); integrations at the edge
  ([[decisions/ADR-0005-integrations-at-the-edge]]).
- `main` receives the package only once the pull request into `dev` is merged with green checks.

## Open / Remaining

- None for the initial package.

## Exact next action

None; the initial package is complete.

## Links

[[00-INDEX]] · [[architecture/TESTING]] · [[CHANGELOG]]

## Referenced by

[[handoffs/_MOC]]
