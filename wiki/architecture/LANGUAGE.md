---
type: language
tags: [architecture]
aliases: [Vocabulary]
---

# Architecture vocabulary

- **Module** — one `.pudu` file and its mirrored page under `wiki/src/`.
- **Kind** — the typed identity of one message, made once and shared. See [[domain/Kind]].
- **Request / Command / Query** — a message answered by exactly one handler; a command answers `()`.
- **Notification** — a message delivered to every handler of its kind. See [[domain/Publication]].
- **Stream** — a request answered by a sequence of items. See [[domain/Stream]].
- **Handler** — the function that answers a message.
- **Behavior** — a layer around the rest of a pipeline. See [[domain/Pipeline]].
- **Processor** — a step before (pre) or after (post) the handler.
- **Exception handler / action** — a component that may answer in place of a failure, or observes it.
- **Open component** — one that applies to every kind. See [[seams/Open]].
- **Route** — every component registered for one kind, before composition.
- **Envelope / Answer** — a message and its outcome carried without naming their types. See [[domain/Erasure]].
- **Slot** — the one place an erased value of a type reads back.
- **Outcome / Failure** — a value or why there is none. See [[domain/Outcome]].

## Referenced by

[[architecture/_MOC]]
