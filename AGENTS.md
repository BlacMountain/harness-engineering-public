# AGENTS.md

This repository maintains the harness method, lifecycle docs, and validation
scripts used to operate this seed repository.

## Startup Protocol

Before changing this repository:

1. Identify the workspace boundary.
2. Identify the repository boundary.
3. Run `git status --short` in the repository root.
4. Read `README.md` and `HARNESS_QUICKSTART.md`.
5. Read task-relevant docs under `docs/`.
6. Only then modify files.

## Documentation Lifecycle

When a task changes behavior, architecture, validation, Git boundaries,
security boundaries, reliability assumptions, or project lifecycle state, update
the relevant docs and validation scripts in the same change.

Use `README.md` and `HARNESS_QUICKSTART.md` to choose the artifact. Keep durable
knowledge and lifecycle records in `docs/`; keep mechanical enforcement in
scripts, lint, tests, hooks, or CI.

## Hard Rules

- MUST NOT run `git init .` until workspace and repository boundaries are clear.
- MUST run `git status --short` before edits in an existing repository.
- MUST preserve user changes and never revert work unless explicitly requested.
- MUST keep `AGENTS.md` short and focused on Agent behavior.
- MUST keep usage workflow details in `HARNESS_QUICKSTART.md`.
- MUST keep docs and manuals concise, task-specific, and free of no-value
  background, slogans, boilerplate, generic capability claims, and repetition.
- MUST tie capability, performance, or effect claims to concrete goals,
  conditions, and measurable evidence; ask before making unsupported claims.
- NEVER commit virtual environments, dependency folders, build output,
  coverage, datasets, checkpoints, experiment outputs, secrets, or local env
  files.

## Seed Repository Checks

For changes to this seed repository, run:

- `scripts/lint`
- `scripts/test`

## Completion Report

Report files changed, harness rules used, verification commands, current git
status, and any remaining user decisions.
