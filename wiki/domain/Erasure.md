---
type: domain
tags: [domain]
---

# Erasure

A value whose type a holder does not name is packed through a slot and read back only through that
slot. Envelopes, answers, routes, composed handlers, and context items all cross untyped
boundaries this way, and none of them can be read as another type. See
[[src/PuduLangMediator/Utils/Erasure]] and [[src/PuduLangMediator/Message]].

## Referenced by

[[architecture/LANGUAGE]] · [[domain/_MOC]]
