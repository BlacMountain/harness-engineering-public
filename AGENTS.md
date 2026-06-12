# AGENTS.md

This repository is a harness guide for creating agent-readable, runnable,
verifiable, constrained, and iterative software projects.

## Required Startup

For any code-targeted task, first read:

1. `.harness/project-policy.yaml`
2. `.harness/workspace-policy.yaml`
3. `.harness/git-policy.yaml`
4. `.harness/quality-policy.yaml`
5. `.harness/architecture-policy.yaml`
6. `README.md`
7. `docs/HARNESS_GUIDE.md`
8. `docs/GIT_POLICY.md`

If `.harness/` is missing in a target project, initialize a minimal policy
layer before writing business code.

## Hard Rules

- MUST identify the workspace type and repository boundary before running git
  initialization or git mutations.
- MUST NOT run `git init .` at a workspace root unless policy explicitly says
  the current directory is a single repository.
- MUST check `git status --short` before editing an existing repository.
- MUST preserve user changes and never revert work that was not explicitly
  requested.
- MUST use an isolated dependency environment for code projects when the policy
  requires it.
- MUST run the available verification commands after changes.
- NEVER commit virtual environments, dependency folders, build output, coverage,
  datasets, model checkpoints, experiment outputs, secrets, or local env files.

## Command Entry Points

- `scripts/setup`
- `scripts/lint`
- `scripts/test`
- `scripts/dev`
- `scripts/harness-check`

If a command cannot apply to the project type, it must exit successfully with a
clear explanation rather than silently doing nothing.

## Completion Report

Every completed task must report:

- Files changed.
- Harness policies used.
- Verification commands run.
- Current git status.
- Remaining risks or user decisions.
