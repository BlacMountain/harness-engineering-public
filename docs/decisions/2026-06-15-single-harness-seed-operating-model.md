# Single Harness Seed Operating Model

## Status

superseded

## Context

Two active execution-plan documents recommended consolidating the repository
from a branching template matrix into one harness operating model. The user
explicitly requested that the project name remain `Harness Seed Repository` and
that conflicts between the plans be left for user decision. The user later
confirmed that the old template matrix and selection document should be deleted.

## Decision

Keep the project name `Harness Seed Repository`.

Use a single four-layer operating model:

- route: `README.md`, `AGENTS.md`, `HARNESS_QUICKSTART.md`
- knowledge: `docs/` and `ARCHITECTURE.md`
- enforcement: `scripts/`, lint, tests, hooks, or CI

Delete the old template matrix and selection document. Project differences must
be expressed as target-local policy deltas, docs, scripts, lint, tests, hooks,
or CI rather than seed-level template branches.

## Alternatives

- Rename the repository to Harness Method Repository or Harness Operating Model
  Repository.
- Keep the old template matrix as optional examples.
- Replace the selection document with target policy profile guidance.
- Keep only docs plus scripts.

## Consequences

- The current startup path no longer requires choosing a seed branch or copying
  a branching template.
- The seed still supports target-local harness creation.
- Reintroducing multiple seed startup paths requires a new architecture decision
  and matching updates to policy and checks.

## Superseded By

`2026-06-16-remove-machine-policy-layer.md` supersedes the machine-policy layer.
The method now uses entry docs, lifecycle docs, and ordinary validation scripts.
