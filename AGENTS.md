# AGENTS.md

This is a Harness Seed Repository. Use it to create target-local harnesses. Do
not use it as the long-term rule source for target projects.

## Startup Protocol

Before changing this repository or a target project:

1. Identify the workspace boundary.
2. Identify the repository boundary.
3. Run `git status --short` in the repository root.
4. Read `README.md`, `HARNESS_QUICKSTART.md`, and `.harness/policy.yaml`.
5. Read task-relevant docs under `docs/`.
6. Only then modify files.

## Target Harness Transfer

When bootstrapping or repairing a target project:

1. Create or update the target-local `README.md`, `AGENTS.md`,
   `ARCHITECTURE.md`, `.harness/`, docs, and validation commands.
2. Put durable target rules in target `.harness/policy.yaml`.
3. Put human explanation, tradeoffs, and operating guidance in target docs.
4. Put mechanical checks in scripts, lint, tests, hooks, or CI.
5. After bootstrap, obey the target project's local rules, not this seed.

## Documentation Lifecycle Rules

When a task changes behavior, architecture, validation, Git boundaries,
security boundaries, or reliability assumptions, update the relevant durable
docs, `.harness/policy.yaml`, and checks in the same change. Use `README.md`
and `HARNESS_QUICKSTART.md` to choose the artifact.

## Hard Rules

- MUST NOT run `git init .` until workspace and repository boundaries are clear.
- MUST run `git status --short` before edits in an existing repository.
- MUST preserve user changes and never revert work unless explicitly requested.
- MUST keep `AGENTS.md` short and route to durable docs and policy.
- MUST keep `.harness/` machine-readable and consumed by checks.
- MUST keep `.harness/policy.yaml` in the constrained YAML subset supported by
  `scripts/harness-check`.
- NEVER commit virtual environments, dependency folders, build output,
  coverage, datasets, checkpoints, experiment outputs, secrets, or local env
  files.

## Seed Repository Checks

For changes to this seed repository, run:

- `scripts/harness-check`
- `scripts/lint`
- `scripts/test`

## Completion Report

Report files changed, harness rules used, verification commands, current git
status, and any remaining user decisions.
