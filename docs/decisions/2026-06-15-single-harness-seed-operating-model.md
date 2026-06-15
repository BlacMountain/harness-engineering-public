# Single Harness Seed Operating Model

## Status

accepted

## Context

Two active execution-plan documents recommended consolidating the repository
from a project-type template matrix into one harness operating model. The user
explicitly requested that the project name remain `Harness Seed Repository` and
that conflicts between the plans be left for user decision. The user later
confirmed that `templates/` and `docs/PROJECT_TYPES.md` should be deleted.

## Decision

Keep the project name `Harness Seed Repository`.

Use a single four-layer operating model:

- route: `README.md`, `AGENTS.md`, `HARNESS_QUICKSTART.md`
- knowledge: `docs/` and `ARCHITECTURE.md`
- machine policy: `.harness/*.yaml`
- enforcement: `scripts/`, lint, tests, hooks, or CI

Retain `.harness/` as machine-readable policy because `scripts/harness-check`
consumes it.

Delete `templates/` and `docs/PROJECT_TYPES.md`. Project differences must be
expressed as target-local policy deltas, docs, scripts, lint, tests, hooks, or
CI rather than seed-level template branches.

## Alternatives

- Rename the repository to Harness Method Repository or Harness Operating Model
  Repository.
- Keep `templates/` as optional examples.
- Replace `docs/PROJECT_TYPES.md` with target policy profile guidance.
- Delete `.harness/` and keep only docs plus scripts.

## Consequences

- The current startup path no longer requires choosing a project type or copying
  a project-type template.
- The seed still supports target-local harness creation.
- `.harness/` must remain small, structured, and consumed by checks.
- Reintroducing multiple seed startup paths requires a new architecture decision
  and matching updates to policy and checks.
