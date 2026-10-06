# Security policy

## Reporting a vulnerability

Report a suspected vulnerability privately through GitHub's
[security advisory form](https://github.com/chrismichaelps/pudu-lang-mediator/security/advisories/new),
or by email to <chrisperezsantiago1@gmail.com> with `SECURITY` in the subject.

Please do not open a public issue for a vulnerability. Include the package version, the `pudu`
version, the platform, and the smallest program that shows the problem.

You can expect an acknowledgement within seven days and a decision on whether the report is
accepted within thirty.

## What is in scope

The package routes caller messages to caller handlers through caller behaviors. A report is in
scope when the mediator routes or runs more, less, or something else than its registrations say:

- A request, notification, or stream delivered to a handler registered for another kind.
- A typed value read back as a type other than the one it was stored as.
- A behavior, processor, or exception component skipped, run twice, or run out of its documented
  order.
- A notification handler that runs after a sequential publication stopped, or a parallel
  publication that answers before every handler has finished.
- A stream reader whose producing thread outlives the reader after it is closed.
- A deadlock or unbounded growth of memory under concurrent use of one mediator.

## What is not in scope

- A handler or behavior that ignores its cancellation token. Cancellation is cooperative.
- Vulnerabilities in the Pudu compiler or standard library; report those to
  [pudu-lang](https://github.com/chrismichaelps/pudu-lang/security), and those of a dependency to
  that package's repository.

## Supported versions

| Version | Supported |
| ------- | --------- |
| 0.1.x   | Yes       |
