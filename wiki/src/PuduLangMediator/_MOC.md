---
type: moc
tags: [moc]
---

# PuduLangMediator

- [[src/PuduLangMediator/Catalog]] — Every registration gathered before a build: one entry per kind holding its packed route and counts, the open components and marks every kind is built with, and the problems found.
- [[src/PuduLangMediator/Compose]] — One kind's pipeline from its route and the open components, composed once at build time, plus the finishers that compose each kind and answer its envelopes.
- [[src/PuduLangMediator/Context]] — What one message carries through its pipeline: the cancellation token every handler and behavior observes, and typed items shared between them.
- [[src/PuduLangMediator/Mediator]] — Routes requests, notifications, and streams to their composed pipelines: builds a mediator from registrations, and sends, publishes, streams, and dispatches envelopes through it.
- [[src/PuduLangMediator/Message]] — What every kind of message shares: its `Info` (name, shape, tags), the `Envelope` a sender that names no types sends, and the `Answer` it gets back.
- [[src/PuduLangMediator/Notification]] — Notifications delivered to every handler: the typed `Kind[N, E]`, named handlers, the `Route` that gathers them, and envelopes and answers.
- [[src/PuduLangMediator/Open]] — Components that apply to every kind: open behaviors, pre-processors, post-processors, exception actions, notification handlers, and stream behaviors, each a trait whose one member is generic over the message types.
- [[src/PuduLangMediator/Publisher]] — What a component that only publishes notifications depends on: a trait the mediator implements, so a test double stands in for it.
- [[src/PuduLangMediator/Publishing]] — How a notification reaches its handlers: the `Strategy` trait over executors and four strategies — sequential, continuing, parallel, and bounded.
- [[src/PuduLangMediator/Reader]] — A stream read one item at a time while another thread produces it, holding a bounded number of items ahead of the consumer.
- [[src/PuduLangMediator/Registration]] — The handlers and components a mediator is built from: one constructor per kind of registration, each a change to the catalog, and `group` to keep related registrations together.
- [[src/PuduLangMediator/Request]] — Requests answered by exactly one handler: the typed `Kind[Q, R, E]`, the handler, behavior, processor, and exception component types, the `Route` that gathers them, and envelopes and answers for untyped dispatch.
- [[src/PuduLangMediator/Sender]] — What a component that only sends requests and streams depends on: a trait the mediator implements, so a test double stands in for it.
- [[src/PuduLangMediator/Stream]] — Requests answered by a sequence of items: the typed `Kind[Q, T, E]`, the sink a handler emits into, stream behaviors that may wrap the sink, and envelopes, items, and answers for untyped streams.
- [[src/PuduLangMediator/Behaviors/_MOC]] — the modules under `PuduLangMediator.Behaviors`.
- [[src/PuduLangMediator/Constants/_MOC]] — the modules under `PuduLangMediator.Constants`.
- [[src/PuduLangMediator/Domain/_MOC]] — the modules under `PuduLangMediator.Domain`.
- [[src/PuduLangMediator/Utils/_MOC]] — the modules under `PuduLangMediator.Utils`.

## Referenced by

[[src/_MOC]]
