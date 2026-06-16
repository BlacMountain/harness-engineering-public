# Remove Machine Policy Layer

## Status

accepted

## Context

The previous design introduced a dedicated machine-readable policy layer and a
dedicated consistency checker. That reduced prose duplication at first, but it
became another rule surface next to `AGENTS.md`, `README.md`, `ARCHITECTURE.md`,
governance docs, and validation scripts.

The public Harness Engineering baseline emphasizes short agent instructions,
repo-local documentation as the record system, execution plans as first-class
project lifecycle artifacts, and mechanical enforcement through ordinary tools
such as lint, tests, hooks, and CI.

## Decision

Remove the dedicated machine policy layer and its dedicated checker.

Use three layers:

- entry: `README.md`, `AGENTS.md`, `HARNESS_QUICKSTART.md`
- lifecycle knowledge: `ARCHITECTURE.md` and `docs/`
- validation: `scripts/`, lint, tests, hooks, and CI

`docs/` is both durable knowledge and project lifecycle management. It owns
specs, active execution plans, completed execution records, technical debt,
decisions, references, and governance documents.

Target-project transfer guidance belongs only in `HARNESS_QUICKSTART.md`.

## Alternatives

- Keep a dedicated policy file and continue validating it.
- Keep an ultra-small machine-entry file only for scripts.
- Move all rules into `AGENTS.md`.

## Consequences

- There is no separate machine policy surface to keep synchronized.
- Human explanation lives in docs; mechanical enforcement lives in ordinary
  validation scripts.
- `AGENTS.md` can stay focused on Agent behavior.
- Reintroducing a dedicated policy layer requires a new decision.
