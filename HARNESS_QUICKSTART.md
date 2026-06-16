# Harness Quickstart

Use this file after `README.md` and `AGENTS.md`. It is the only place in this
seed repository that explains how to apply the harness method to a target
project.

## 1. Confirm Boundaries

Before any code, documentation, or Git work:

1. Identify the workspace boundary.
2. Identify the repository boundary with `git rev-parse --show-toplevel` when
   inside an existing repository.
3. Run `git status --short`.
4. Check whether the target project already has local `AGENTS.md`, docs, and
   validation commands.

If the boundary is unclear, stop and ask for the intended target repository. Do
not run `git init .` by default.

## 2. Identify The Work Stage

| Situation | Next artifact |
| --- | --- |
| Need to define goals, users, behavior, or acceptance | `docs/product-specs/` |
| Need to implement a non-trivial change | `docs/exec-plans/active/` |
| Need to record a durable technical or governance choice | `docs/decisions/` |
| Need to rely on an external API, paper, platform, law, or protocol | `docs/references/` |
| Need to change repository, artifact, or Git boundaries | `docs/GIT_POLICY.md` |
| Need to change setup, lint, test, dev, or done criteria | `docs/QUALITY.md` and validation scripts |
| Need to change failure, recovery, logs, timeout, or rollback behavior | `docs/RELIABILITY.md` |
| Need to change secret, auth, permission, input, or trust boundaries | `docs/SECURITY.md` |
| Need to enforce a repeated rule mechanically | scripts, lint, tests, hooks, or CI |

## 3. Target Harness Transfer

For a long-lived target project, create or update only the target-local artifacts
needed by the task:

- `README.md`
- `AGENTS.md`
- `ARCHITECTURE.md`
- `docs/product-specs/index.md`
- `docs/exec-plans/active/`
- `docs/exec-plans/completed/`
- `docs/exec-plans/tech-debt-tracker.md`
- `docs/decisions/index.md`
- `docs/references/index.md`
- `docs/GIT_POLICY.md`
- `docs/QUALITY.md`
- `docs/RELIABILITY.md`
- `docs/SECURITY.md`
- `scripts/setup`
- `scripts/lint`
- `scripts/test`
- `scripts/dev` or `scripts/run`

Code projects must use an isolated dependency environment. Python projects
should use `.venv`; other ecosystems should use their normal local isolation.
Pure documentation tasks do not require creating an environment.

## 4. Transfer Rules

The target project owns its long-term rules. After bootstrap:

1. Read the target project's `AGENTS.md`.
2. Read the target project's task-relevant docs.
3. Run the target project's validation commands.
4. Put durable knowledge and lifecycle records in target-local docs.
5. Put mechanical checks in target-local scripts, lint, tests, hooks, or CI.
6. Stop depending on this seed repository for day-to-day development.
