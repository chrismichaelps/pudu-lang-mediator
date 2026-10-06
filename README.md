<p align="center">
  <img src="public/pudu-lang-short.png" alt="Pudu" width="120">
</p>

<p align="center">
  <a href="https://www.pudu-lang.org/">Pudu</a> |
  <a href="https://www.pudu-lang.org/docs">Documentation</a> |
  <a href="https://www.pudu-lang.org/packages">Packages</a> |
  <a href="https://github.com/chrismichaelps/pudu-lang-mediator/wiki">API docs</a> |
  <a href="CONTRIBUTING.md">Contributing</a>
</p>

# pudu-lang-mediator

In-process messaging for Pudu. Parts of a program talk through a mediator instead of calling each
other: a request is answered by exactly one handler, a notification reaches every handler of its
kind, and a stream request answers a sequence of items. Every message runs through a pipeline of
behaviors, processors, and exception components composed once when the mediator is built, and
every answer is an `Outcome`: a value, or a `Failure` saying why there is none.

```pudu
import PuduLangMediator as Messaging
import PuduLangMediator.Context as Context
import PuduLangMediator.Mediator as Mediator
import PuduLangMediator.Registration as Registration
import PuduLangMediator.Request as Request

type GetPrice = { item: Str }

fn main() -> Int {
  let getPrice: Request.Kind[GetPrice, Int, Str] = Request.kind("prices.get")
  let mediator = match Mediator.build([
      Registration.handler(&getPrice, fn(request: GetPrice, _context: Context.Context) -> Messaging.Outcome[Int, Str] {
          if request.item == "tea" { Ok(4) } else { Messaging.raise("no price for " + request.item) }
        })
    ]) {
    case Ok(built) => built
    case Err(invalid) => panic(Mediator.explain(&invalid))
  }
  let price = Mediator.send(&mediator, &getPrice, GetPrice{item: "tea"})
  if price == Ok(4) { 0 } else { 1 }
}
```

## Installing

```bash
pudu install @chrismichaelps/pudu-lang-mediator
```

It needs [Pudu 0.1.3 or later](https://www.pudu-lang.org/download). Every module the package ships
is `PuduLangMediator` or under it, so it takes no module name from the program that installs it.

## Concepts

| Concept | Module | What it is |
| --- | --- | --- |
| Outcome and failure | `PuduLangMediator` | `Outcome[T, E]` is `Result[T, Failure[E]]`. `Failure` is `Raised(E)`, `Unhandled`, `Mismatched`, `Refused`, `Invalid` with violations, `Cancelled`, `Crashed`, or `Aggregate`. |
| Kinds | `Request`, `Notification`, `Stream` | The typed identity of a message: `Request.kind` (a query answering `R`), `Request.command` (answering `()`), `Notification.kind`, `Stream.kind`. Make each once and share it. |
| Registration | `Registration` | Handlers, behaviors, processors, and exception components for one kind, or open ones for every kind, as one flat array. |
| Mediator | `Mediator` | `build` and `buildWith` report every registration problem at once; `send`, `resolve`, `publish`, `stream`, `collect`, `reader`, `dispatch`, `broadcast`, `streamEnvelope`. |
| Context | `Context` | The cancellation token every layer observes and typed items shared between layers. |
| Open components | `Open` | Traits for behaviors, processors, exception actions, notification handlers, and stream behaviors that apply to every kind. |
| Publishing | `Publishing` | `Sequential` (the default), `Continuing`, `Parallel`, `Bounded`, or your own `Strategy`. |
| Seams | `Sender`, `Publisher` | Traits the mediator implements, so a component depends only on what it does and a test double stands in. |
| Untyped dispatch | `Message` | Envelopes and answers for code that routes messages without naming their types. |

## Pipelines

Each request kind runs through the same order, outermost first, and a layer with no components is
left out:

1. Exception actions and exception handlers. Actions observe only failures no handler answered by
   default (`Catalog.ForUnhandled`); with `Catalog.ForAll` they observe every failure.
2. Pre-processors, in registration order.
3. Post-processors, in registration order, after a value.
4. Behaviors, open and per kind, in registration order.
5. The handler.

Stream kinds run their stream behaviors, which may wrap the sink to filter, map, or count items.

## Integrations

Three behaviors build on other Pudu packages and are the only modules that import them:

| Module | Package | What it adds |
| --- | --- | --- |
| `Behaviors.Validation` | `pudu-lang-validator` | answers `Invalid` with every violation before the handler runs |
| `Behaviors.Logging` | `pudu-lang-log` | a structured event at the start and end of every request, stream, and notification |
| `Behaviors.Resilient` | `pudu-lang-resilience` | retries, timeouts, circuit breaking, rate limiting, hedging, and fallback around a kind's pipeline |

## Examples

`examples/` holds runnable programs: `QuickStart`, `PipelineBehaviors`, `ExceptionHandling`,
`Notifications`, `Streaming`, `UntypedDispatch`, and `Integrations`.

```bash
pudu run examples/QuickStart.pudu
```

## Developing

```bash
pudu install --locked                                 # the integration packages
pudu test test                                        # every suite
pudu fmt --check src test tools examples && pudu lint src test tools examples
pudu run tools/Mutate.pudu --domain --threshold 100   # mutation testing of the pure layer
```

The design lives in the [wiki vault](wiki/00-INDEX.md): one page per source file, the decisions
behind the pipeline, and the Pudu grammar rules the code follows. The
[API docs](https://github.com/chrismichaelps/pudu-lang-mediator/wiki) walk through every module.

## License

[Apache License 2.0](LICENSE).
