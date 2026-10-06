---
type: module
path: "@root/src/PuduLangMediator/Behaviors/Logging.pudu"
fidelity: Active
grammar: "[[grammar/pudu]]"
depth_score: 0.6
depth_status: MEDIUM
tags: [module, integration]
aliases: [PuduLangMediator.Behaviors.Logging]
---

# PuduLangMediator.Behaviors.Logging

## Purpose

Structured events for every request, stream, and notification through a `pudu-lang-log` logger: one recorder implements the open behavior, open stream behavior, and open notification handler.

## Interface

### Signatures

```pudu
export type Recorder = { logger: Logger.Logger }

export const STARTED: Str

export const SUCCEEDED: Str

export const STREAMED: Str

export const FAILED: Str

export const HEARD: Str

export fn of(logger: Logger.Logger) -> Recorder
```

### Implementations

- `impl Open.Behavior for Recorder`
- `impl Open.StreamBehavior for Recorder`
- `impl Open.NotificationHandler for Recorder`

### Linkage

- **Requires:** [[src/PuduLangMediator]], [[src/PuduLangMediator/Context]], [[src/PuduLangMediator/Message]], [[src/PuduLangMediator/Open]], [[src/PuduLangMediator/Request]], [[src/PuduLangMediator/Stream]], [[src/PuduLangMediator/Utils/Shared]]; packages: `PuduLangLog.Logger`, `PuduLangLog.Value`.
- **Consumed by:** programs using the package.

## Algorithm

1. A debug event names the kind and shape at the start; an information event names the elapsed milliseconds (and items for a stream) on success; a warning event carries the failure's description.
2. Elapsed time is read from the monotonic clock.

## Negative Logic (Prohibited Paths)

- Logging never changes an outcome.

## Edge Cases

- Registered first, the recorder sees retries and recoveries made by inner layers as one message.

## Depth

DEPTH 0.6 (MEDIUM). Tested by the suite mirroring this module under `test/`.

## Grill Log

- **Q:** Why message templates as public constants?
  **A:** Tests and log queries select events by template; constants keep them exact. _Rejected:_ inline templates.

## Referenced by

[[seams/Open]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Behaviors/_MOC]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Message]] · [[src/PuduLangMediator/Open]] · [[src/PuduLangMediator/Request]] · [[src/PuduLangMediator/Stream]] · [[src/PuduLangMediator/Utils/Shared]] · [[subsystems/Integrations]]
