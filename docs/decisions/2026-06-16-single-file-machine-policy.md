# Single File Machine Policy

## Status

accepted

## Context

The seed repository had five machine-readable policy files under `.harness/`:
project, workspace, git, quality, and architecture. That split was readable but
created unnecessary entry points for the current single harness operating model.
Only `scripts/harness-check` consumed the files, so multiple policy files added
maintenance cost without a matching enforcement benefit.

The repository should keep `.harness/` as the machine policy layer, but the
machine contract should have one canonical entry point.

## Decision

Use `.harness/policy.yaml` as the only machine-readable policy file.

The policy file uses a constrained YAML subset that `scripts/harness-check` can
parse without PyYAML or other external dependencies: simple mappings, simple
lists, plain scalar values, no anchors, aliases, multiline scalars, complex
nesting, or duplicate keys.

Delete the legacy five policy files and reject them in `scripts/harness-check`.
Delete `docs/examples/policies/` because those examples belonged to the old
branching model.

## Alternatives

- Keep the five policy files and update documentation only.
- Keep the five files as compatibility shims while adding `.harness/policy.yaml`.
- Remove `.harness/` entirely and make scripts validate docs directly.

## Consequences

- Agents have one machine-policy file to read before changing the seed or a
  target harness.
- `scripts/harness-check` has one policy input and must stay aligned with the
  constrained YAML shape.
- Target-local differences should be expressed as changes inside the target's
  `.harness/policy.yaml`, docs, scripts, lint, tests, hooks, or CI.
- Reintroducing multiple policy files requires a new decision and matching
  updates to checks and docs.
