---
type: domain
tags: [domain]
---

# Stream

A stream handler pushes items into a sink until the sink declines one. A consumer takes them with
a sink, collects them, or pulls them one at a time through a reader that runs the stream on its own
thread behind a bounded channel. See [[src/PuduLangMediator/Stream]] and
[[src/PuduLangMediator/Reader]].

## Referenced by

[[architecture/LANGUAGE]] · [[domain/_MOC]]
