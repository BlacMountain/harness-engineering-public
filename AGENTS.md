# AGENTS.md

This is a Harness Seed Repository. Use it to bootstrap target projects, not as
the long-term source of rules for those projects.

## Startup Protocol

Before doing any code work in a target project:

1. Identify the workspace boundary.
2. Identify the repository boundary.
3. Identify the project type.
4. Check whether the target project has `.harness/`.
5. If `.harness/` is missing, initialize from `templates/<project-type>/`.
6. Read the target project's `AGENTS.md` and `.harness/*.yaml`.
7. Only then modify code.

## Project Types

- `source-repo`: product, service, CLI, library, or application source.
- `infra-repo`: Terraform, Ansible, Helm, Kubernetes, Docker Compose, or infra.
- `research-repo`: experiments, training code, configs, and reproducible runs.
- `deployment-repo`: deployment orchestration, release scripts, env templates.
- `knowledge-repo`: docs, policy, guidelines, and reusable operational knowledge.

Use `HARNESS_QUICKSTART.md` when project type or workspace type is unclear.

## Hard Rules

- MUST NOT run `git init .` until workspace and repository boundaries are clear.
- MUST NOT rely on this seed repository as the long-term policy for a target.
- MUST copy or generate target-local `AGENTS.md`, `.harness/`, and checks.
- MUST run `git status --short` before edits in an existing repository.
- MUST preserve user changes and never revert work unless explicitly requested.
- NEVER commit virtual environments, dependency folders, build output, coverage,
  datasets, checkpoints, experiment outputs, secrets, or local env files.

## Seed Repository Checks

For changes to this seed repository, run:

- `scripts/harness-check`
- `scripts/lint`
- `scripts/test`

For docs changes in this seed repository, route through root `README.md` and
the target docs directory README. Keep detailed docs rules out of this file.

## Completion Report

Report files changed, policies used, verification commands, git status, and any
remaining user decisions.
