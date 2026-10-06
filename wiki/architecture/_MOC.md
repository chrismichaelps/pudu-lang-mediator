---
type: moc
tags: [moc, architecture]
aliases: [Architecture]
---

# Architecture

## Shape

A program makes each message kind once, registers handlers and components against kinds in an
array, and builds a [[src/PuduLangMediator/Mediator|mediator]]. The build gathers the registrations
in a [[src/PuduLangMediator/Catalog|catalog]], refuses it with every problem found, and otherwise
[[src/PuduLangMediator/Compose|composes]] each kind's pipeline once. Sending a message is then a
lookup by name, an [[src/PuduLangMediator/Utils/Erasure|unpack]] through the kind's own slot, and a
call.

Every layer answers an `Outcome`, so a refusal by one layer is a value the layers outside it judge
like any other.

## Layers

| Layer | Holds | May import |
| --- | --- | --- |
| `Constants/` | message templates | nothing |
| `Utils/` | erasure, shared state, templates, thread reasons | std, Constants |
| `Domain/` | pure ordering and registration census | Utils, Constants |
| public modules | vocabulary, kinds, context, open components, publishing, catalog, composition, registration, mediator, reader, traits | Domain, Utils, Constants, each other, std |
| `Behaviors/` | validation, logging, and resilience integrations | public modules, std, the three integration packages |

`Domain/` performs no effects and imports no public module. Nothing outside `Behaviors/` imports
another package ([[decisions/ADR-0005-integrations-at-the-edge]]).

## Pages

- [[architecture/LANGUAGE]] — the vocabulary every page uses.
- [[architecture/TESTING]] — the test levels and what each proves.
- [[grammar/pudu]] — the language rules the code follows.

## Referenced by

[[00-INDEX]]
