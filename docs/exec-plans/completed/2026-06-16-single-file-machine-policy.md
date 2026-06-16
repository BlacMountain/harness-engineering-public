# Single File Machine Policy Execution

## Background

The seed repository now uses one unified harness operating model. Keeping five
separate machine policy files and old branching examples made the Agent entry
path longer than necessary and risked reintroducing obsolete bootstrap choices.

## Goal

Consolidate the machine-readable policy layer into one file, shorten the Agent
entry rules, remove obsolete examples, and keep checks and docs aligned with
the new single policy contract.

## Completed Changes

- Added one policy file with structure, command, workspace, Git, quality, and
  architecture rules in a constrained YAML subset.
- Updated the dedicated checker to consume only the single policy file and to
  reject legacy policy file names.
- Removed the previous split policy files and old example policy files.
- Updated route, architecture, quality, Git, security, product-spec, reference,
  and Codex instruction docs to point to the single policy file.
- Marked the previous machine-policy file-layout decision as superseded and
  added an accepted decision for the single policy file.

## Verification

Passed:

- the dedicated consistency checker
- `scripts/lint`
- `scripts/test`
- fixed-string stale reference scans for old policy paths and removed model
  identifiers
