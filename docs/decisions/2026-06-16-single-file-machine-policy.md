# Single File Machine Policy

## Status

superseded

## Context

The seed repository had five machine-readable policy files for project,
workspace, Git, quality, and architecture rules. That split was readable but
created unnecessary entry points for the current single harness operating model.
Only the dedicated consistency checker consumed the files, so multiple policy
files added maintenance cost without a matching enforcement benefit.

The repository should keep a machine policy layer with one canonical entry point.

## Decision

Use one machine-readable policy file.

The policy file uses a constrained YAML subset that the dedicated consistency
checker can parse without PyYAML or other external dependencies: simple
mappings, simple lists, plain scalar values, no anchors, aliases, multiline
scalars, complex nesting, or duplicate keys.

Delete the legacy five policy files and reject them in the checker. Delete the
old example policy files because those examples belonged to the old branching
model.

## Alternatives

- Keep the five policy files and update documentation only.
- Keep the five files as compatibility shims while adding one policy file.
- Remove the machine policy layer entirely and make scripts validate docs directly.

## Consequences

- Agents have one machine-policy file to read before changing the seed or a
  target harness.
- The dedicated consistency checker has one policy input and must stay aligned
  with the constrained YAML shape.
- Target-local differences should be expressed as changes inside the target's
  docs, scripts, lint, tests, hooks, or CI.
- Reintroducing multiple policy files requires a new decision and matching
  updates to checks and docs.

## Superseded By

`2026-06-16-remove-machine-policy-layer.md` removes the dedicated machine policy
layer and replaces it with lifecycle docs plus ordinary validation scripts.
