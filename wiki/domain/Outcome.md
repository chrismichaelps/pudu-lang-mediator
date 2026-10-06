---
type: domain
tags: [domain]
---

# Outcome

Every handler, behavior, processor, and publication answers `Result[T, Failure[E]]`. A failure is
the handler's own error, a missing or mismatched route, a refusal, a validation failure, a
cancellation, a crash, or several of those combined. See [[src/PuduLangMediator]] and
[[decisions/ADR-0001-outcomes-not-exceptions]].

## Referenced by

[[architecture/LANGUAGE]] · [[domain/_MOC]]
