---
type: domain
tags: [domain]
---

# Kind

A kind is the typed identity of one message: `Request.Kind[Q, R, E]`, `Notification.Kind[N, E]`,
or `Stream.Kind[Q, T, E]`. It carries a name, a shape, tags, and the slots its routes, composed
handlers, messages, and outcomes are erased through. A program makes each kind once and shares it
between the registrations and the senders; two kinds made under one name are different kinds, and
a build refuses them. See [[src/PuduLangMediator/Request]], [[src/PuduLangMediator/Notification]],
[[src/PuduLangMediator/Stream]], and [[decisions/ADR-0002-typed-kinds]].

## Referenced by

[[architecture/LANGUAGE]] · [[decisions/ADR-0002-typed-kinds]] · [[domain/_MOC]]
