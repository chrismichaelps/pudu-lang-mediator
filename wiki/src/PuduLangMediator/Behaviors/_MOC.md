---
type: moc
tags: [moc]
---

# PuduLangMediator.Behaviors

- [[src/PuduLangMediator/Behaviors/Logging]] — Structured events for every request, stream, and notification through a `pudu-lang-log` logger: one recorder implements the open behavior, open stream behavior, and open notification handler.
- [[src/PuduLangMediator/Behaviors/Resilient]] — Requests run through a `pudu-lang-resilience` pipeline: retries, timeouts, circuit breaking, rate limiting, hedging, and fallback around the rest of a kind's pipeline.
- [[src/PuduLangMediator/Behaviors/Validation]] — Requests and streams refused when their rules fail: a per-kind behavior that runs a `pudu-lang-validator` validator and answers `Invalid` with every violation.

## Referenced by

[[src/PuduLangMediator/_MOC]]
