# Contributing to pudu-lang-mediator

The wiki vault under `wiki/` is the source of truth. Read `wiki/00-INDEX.md`, the architecture
map, and the grammar page before changing code, and write or update a module's mirrored page under
`wiki/src/` before its code.

## Branches

- `main` holds released versions only. It changes through a pull request from `dev`.
- `dev` is the integration branch.
- Work happens on `feature/<issue>-<slug>`, `fix/<issue>-<slug>`, or `docs/<issue>-<slug>`,
  branched from `dev` and merged back through a pull request.

## Commits

Semantic commits that name the issue: `feat(notification): add a bounded publisher refs #12`.
Types are `feat`, `fix`, `perf`, `refactor`, `test`, `docs`, `ci`, and `chore`. Keep each commit
to one change; code, tests, and the matching wiki pages move together.

## Code

- Dependencies point inward: the public modules use `Domain`, which uses `Utils` and
  `Constants`. `Domain` performs no effects and imports no public module.
- The mediator core imports no other package. Only the modules under `Behaviors/` use the
  validator, log, and resilience packages, and nothing in the core imports them.
- Every module the package ships is `PuduLangMediator` or lives under `src/PuduLangMediator/`.
- Every file and exported type starts with a one-line `/** @Namespace.Entity.Role — intent */`
  anchor, and every function and constant carries a short `///` comment saying what it answers.
- No dead code: every declaration has a caller.
- Keep files under 500 lines.

## Checks

Every change passes all of these:

```bash
pudu install --locked
pudu check $(find src test tools examples -name '*.pudu')
pudu fmt --check src test tools examples
pudu lint src test tools examples
pudu test test
```

Changes to logic also run mutation testing on the files they touch, and no new mutant may survive
without a written reason:

```bash
pudu run tools/Mutate.pudu --file src/PuduLangMediator/Domain/Order.pudu
```

## Pull requests

Open the pull request against `dev`, describe the behaviour and how it was verified, and link the
issue with `Closes #N`.
