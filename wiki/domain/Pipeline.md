---
type: domain
tags: [domain]
---

# Pipeline

The layers around one request kind's handler, outermost first: exception actions and exception
handlers (in the order the scope sets), pre-processors, post-processors, then every behavior, open
and the kind's own, in registration order. A layer with no components is left out. A stream kind's
pipeline is its stream behaviors in registration order. See [[src/PuduLangMediator/Compose]] and
[[decisions/ADR-0003-pipeline-order]].

## Referenced by

[[architecture/LANGUAGE]] · [[decisions/ADR-0003-pipeline-order]] · [[domain/_MOC]]
