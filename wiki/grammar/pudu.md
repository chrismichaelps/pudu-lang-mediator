---
type: grammar
language: Pudu
version: "0.1.3"
tags: [grammar]
aliases: [Grammar — Pudu, Pudu Grammar]
---

# Grammar — Pudu

The Pudu surface this repository is written against, pinned to compiler `0.1.3` as published in
its release archive. Where this page and the compiler disagree, the compiler wins and this page is
corrected in the same change.

## SDK Discovery Map

| Need | Module | Entry points |
| --- | --- | --- |
| Threads | `Std.Concurrent` | `start`, `join`, `contain`, `parallelBounded`, `sleep` |
| Cancellation | `Std.Concurrent.Cancel` | `token`, `child`, `childExpiring`, `cancel`, `stopped`, `check`, `pause`, `explain` |
| Shared state | `Std.Sync` | `mutex`, `withLock`, `cell`, `get`, `set`, `swap` |
| Channels | `Std.Channel` | `channel`, `send`, `receive`, `close` |
| Hash lookup | `Std.HashMap` | `empty`, `get`, `insert`, `remove`, `values`, `keys` (insertion order) |
| Monotonic time | `Std.Time` | `elapsed` |
| Integer bounds | `Std.Math` | `min`, `max` |
| Options and results | `Std.Option`, `Std.Result` | `unwrapOr`, `andThen` |
| Tests | `Std.Test` | `suite`, `equals`, `that`, `not`, `run`, `failuresOf`, `report` |

## Imports / Namespaces

- One module per file; the module name is the path under its source root with `/` as `.`.
- Every import is qualified and aliased. The package root is imported `as Messaging` so the
  `Mediator` alias stays free for `PuduLangMediator.Mediator`.
- Suites under `test/` and programs under `examples/` import package modules through the
  manifest's source root; dependencies resolve from `deps/` after `pudu install --locked`.

## Core Primitives

- Records, sum types, generic aliases of function types, and `Type{..base, field: value}` updates.
- `dynamic Trait` holds any implementation; a trait member may be generic and is still callable
  through a dynamic value. Widening is implicit; there is no narrowing, no runtime type test, and
  no cast.
- A value crosses an untyped boundary through [[src/PuduLangMediator/Utils/Erasure]]: a closure
  that writes into its slot's cell, read back only through that slot.
- A closure captures a copy of every binding it names; a `Sync.Cell` copied this way is still the
  same cell. State callbacks must observe lives in a cell, guarded by a mutex when a write depends
  on a read ([[src/PuduLangMediator/Utils/Shared]]).
- Module scope holds only `const`, folded at compile time; nothing that opens a runtime resource
  (a cell, a mutex, a slot) can be one.

## Architectural Laws

- Dependency direction is inward: public modules use `Domain`, which uses `Utils` and `Constants`.
- Failures are values: every handler and component answers an `Outcome`.
- Every module the package ships is `PuduLangMediator` or lives under `src/PuduLangMediator/`.
- Only `Behaviors/` imports another package.

## Syntax Rules / Naming

- Types, traits, modules, and variants are `PascalCase`; values `camelCase`; constants
  `UPPER_SNAKE_CASE`.
- Every file header and exported type carries the FMCF anchor, one line:
  `/** @Namespace.Entity.Role — intent */`.
- Every `fn`, trait member, and `const` carries a `///` doc comment stating what it answers or
  holds. Rationale belongs in the mirrored page's Grill Log.

## Prohibited Patterns (verified against the 0.1.3 compiler)

- **`scope`, `module`, `where`, and `task` are keywords** and cannot name a binding or a field;
  the exception-action option is `actionScope`.
- **Two traits with a member of the same name implemented by one type are ambiguous** where the
  value is widened to one of them; every open trait's member has its own name.
- **A parameter named like a function of the same module shadows it** (`W2001`); kind parameters
  are named `subject`.
- **Two statements on one line are an error** (`E1049`); there is no statement separator.
- **A tuple is not an assignable place** (`E3077`); swap through a named binding.
- **A match arm that only rebuilds its failure is reported** (`W3003`); use `?` or
  `Result.andThen`.
- **A brace inside a string literal is interpolation**; a literal brace is `\{` or `\}`.
- **`Array.get(i)` and `items[i]` stop the program when `i` is out of range**; use `List.first`.

## Senior Definition Needed

(none open)

## Referenced by

[[00-INDEX]] · [[architecture/_MOC]] · [[src/PuduLangMediator]] · [[src/PuduLangMediator/Behaviors/Logging]] · [[src/PuduLangMediator/Behaviors/Resilient]] · [[src/PuduLangMediator/Behaviors/Validation]] · [[src/PuduLangMediator/Catalog]] · [[src/PuduLangMediator/Compose]] · [[src/PuduLangMediator/Constants/Messages]] · [[src/PuduLangMediator/Context]] · [[src/PuduLangMediator/Domain/Census]] · [[src/PuduLangMediator/Domain/Order]] · [[src/PuduLangMediator/Mediator]] · [[src/PuduLangMediator/Message]] · [[src/PuduLangMediator/Notification]] · [[src/PuduLangMediator/Open]] · [[src/PuduLangMediator/Publisher]] · [[src/PuduLangMediator/Publishing]] · [[src/PuduLangMediator/Reader]] · [[src/PuduLangMediator/Registration]] · [[src/PuduLangMediator/Request]] · [[src/PuduLangMediator/Sender]] · [[src/PuduLangMediator/Stream]] · [[src/PuduLangMediator/Utils/Erasure]] · [[src/PuduLangMediator/Utils/Shared]] · [[src/PuduLangMediator/Utils/Template]] · [[src/PuduLangMediator/Utils/Threads]] · [[tools/Mutate]]
