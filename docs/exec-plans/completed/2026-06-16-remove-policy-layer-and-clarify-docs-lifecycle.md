# Remove Policy Layer And Clarify Docs Lifecycle

## Background

The seed repository no longer needs a dedicated machine policy layer or a
dedicated consistency checker. The repo should match the simpler Harness model:
short Agent behavior instructions, lifecycle docs as the durable record system,
and ordinary scripts for validation.

## Completed Changes

- Removed the dedicated machine policy surface and dedicated consistency checker.
- Updated `scripts/lint` and `scripts/test` to validate the repository without a
  dedicated checker.
- Reframed `docs/` as durable knowledge and project lifecycle management.
- Rewrote `README.md`, `AGENTS.md`, `HARNESS_QUICKSTART.md`, and
  `ARCHITECTURE.md` around the three-layer model.
- Removed duplicate guide/custom-instruction docs whose remaining content was
  covered by the entry, architecture, lifecycle, and governance docs.
- Updated governance docs, product specs, references, decisions, and technical
  debt records for the new model.

## Verification

Passed:

- `scripts/lint`
- `scripts/test`
- stale-reference scans for removed policy/check surfaces in current docs and
  scripts
