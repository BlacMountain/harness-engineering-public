# AGENTS.md

This is a Harness Seed Repository. Use it to bootstrap target projects. Do not
use it as the long-term rule source for target projects.

## Startup Protocol

Before doing code work in a target project:

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
- `knowledge-repo`: docs, policy, guidelines, and operational knowledge.

Use `HARNESS_QUICKSTART.md` when project type or workspace type is unclear.

## Target Harness Transfer

When bootstrapping a target project from this seed:

1. Copy or generate the target-local `AGENTS.md`, `.harness/`, docs, and checks.
2. Use root `README.md` to map seed principles to the target project structure.
3. Use the relevant docs directory `README.md` to create target-local docs.
4. Put durable target rules in target `.harness/*.yaml` before explaining them in docs.
5. Add or update the target `scripts/harness-check` for rules that must be enforced.

## Hard Rules

- MUST NOT run `git init .` until workspace and repository boundaries are clear.
- MUST NOT rely on this seed repository as target-project long-term policy.
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

## Completion Report

Report files changed, policies used, verification commands, git status, and any
remaining user decisions.
