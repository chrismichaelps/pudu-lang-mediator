---
type: architecture
tags: [architecture, test]
aliases: [Testing]
---

# Testing

Every suite is a file under `test/`, mirroring the module it covers; `pudu test test` runs them all
and each suite names its failed checks on stderr.

| Level | Suites | What they prove |
| --- | --- | --- |
| Domain | `test/PuduLangMediator/Domain/**`, `test/PuduLangMediator/Utils/**` | ordering picks, census boundaries, erasure isolation under concurrent reads, templates, atomic shared updates |
| Vocabulary and kinds | the root suite, `MessageTest`, `RequestTest`, `NotificationTest`, `StreamTest`, `ContextTest` | outcomes, combination, descriptions, envelopes, answers, typed items, cancellation |
| Pipeline | `ComposeTest`, `RegistrationTest`, `CatalogTest` | the documented order of every layer, both exception scopes, short-circuits, context hand-off, sink wrapping, counting and collisions |
| Mediator | `MediatorTest`, `SenderTest`, `PublisherTest` | building and every refusal, sending, resolving, publishing, streaming, untyped dispatch, test doubles |
| Concurrency | `PublishingTest`, `ReaderTest`, `Utils/ErasureTest`, `Utils/UtilsTest` | real threads: parallel peaks, bounded worker limits, crash containment, back-pressure, closing joins the producer |
| Integrations | `test/PuduLangMediator/Behaviors/**` | validation violations, log events and levels, retries, timeouts, and cancellation through a resilience pipeline |
| Package | `test/Package/LayoutTest` | every shipped module is the root `PuduLangMediator` or under it, is named after its path, and agrees with the manifest |
| Vault | `test/Package/VaultTest` | the vault mirrors `src/` page for page, every page has a Grill Log, every exported function is in its page's signatures, every link resolves, and every page lists the pages linking to it |
| Integration | `test/Integration/OrderingScenarioTest` | one shop wired through validation, logging, parallel publication, a handler publishing through its own mediator, queries, untyped dispatch, and a reader |
| Examples | `examples/*.pudu`, run by CI | the documented programs compile and answer 0 |
| Mutation | [[tools/Mutate]] | the suites notice single-point changes to the pure layer |

The mutation gate runs on pull requests over `Domain/` with a threshold of 100: every valid mutant
is killed (12 of 12 at the initial package).

## Referenced by

[[CHANGELOG]] · [[architecture/_MOC]] · [[handoffs/2026-10-06-initial-package]]
