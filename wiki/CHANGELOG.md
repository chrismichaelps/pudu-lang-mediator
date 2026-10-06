---
type: changelog
tags: [changelog]
---

# Changelog

## 2026-10-06 — Initial package (#1)

- The package `@chrismichaelps/pudu-lang-mediator` 0.1.0 with the module root `PuduLangMediator`
  and the language range `>=0.1.3 <0.2.0`.
- Core vocabulary: `Failure`, `Outcome`, and `Violation` ([[src/PuduLangMediator]],
  [[decisions/ADR-0001-outcomes-not-exceptions]]); typed kinds for requests, commands,
  notifications, and streams ([[src/PuduLangMediator/Request]], [[src/PuduLangMediator/Notification]],
  [[src/PuduLangMediator/Stream]], [[decisions/ADR-0002-typed-kinds]]).
- Values cross untyped boundaries through slots, never casts ([[src/PuduLangMediator/Utils/Erasure]]).
- Pipelines composed once per kind in one documented order: exception actions and handlers,
  pre-processors, post-processors, then behaviors in registration order
  ([[src/PuduLangMediator/Compose]], [[decisions/ADR-0003-pipeline-order]]).
- Open components for every layer ([[src/PuduLangMediator/Open]]) and four publishing strategies
  ([[src/PuduLangMediator/Publishing]]).
- Explicit registration validated as a whole at build time ([[src/PuduLangMediator/Registration]],
  [[src/PuduLangMediator/Mediator]], [[decisions/ADR-0004-explicit-registration]]).
- Untyped dispatch of requests, notifications, and streams through envelopes; the
  [[src/PuduLangMediator/Sender]] and [[src/PuduLangMediator/Publisher]] traits; a pulled stream
  reader ([[src/PuduLangMediator/Reader]]); typed context items ([[src/PuduLangMediator/Context]]).
- Integration behaviors for validation, logging, and resilience, kept out of the core
  ([[subsystems/Integrations]], [[decisions/ADR-0005-integrations-at-the-edge]]).
- Suites for every module, a package layout test, a vault test, an integration scenario, runnable
  examples, and mutation testing of the pure layer, which kills 12 of 12 mutants
  ([[architecture/TESTING]]).

## Referenced by

[[00-INDEX]] · [[handoffs/2026-10-06-initial-package]]
