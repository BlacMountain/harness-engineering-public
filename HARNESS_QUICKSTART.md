# Harness Quickstart

This repository is a seed for creating target-local harnesses. The seed is used
at bootstrap time; the target project must own its long-term rules through its
own `AGENTS.md`, `.harness/*.yaml`, and validation commands.

## Decision Tree

### 1. Find The Workspace Boundary

Check the current directory and parents:

- If the directory contains one project and one git root, use `single-repo`.
- If one git root contains multiple packages or services, use `mono-repo`.
- If one workspace contains multiple independent git repositories, use
  `mono-workspace`.

If unclear, stop and ask for the intended target repository. Do not run
`git init .`.

### 2. Find The Repository Boundary

Run `git rev-parse --show-toplevel` when inside an existing repository. If there
is no git root, use `.harness/workspace-policy.yaml` or user confirmation before
initializing one.

### 3. Choose Project Type

- Use `source-repo` for product code, services, CLIs, libraries, and apps.
- Use `infra-repo` for Terraform, Ansible, Helm, Kubernetes, Docker Compose, and
  system configuration.
- Use `research-repo` for experiments, model training, analysis pipelines, and
  reproducible research.
- Use `deployment-repo` for deployment orchestration, release automation, and
  environment templates.
- Use `knowledge-repo` for docs, standards, runbooks, prompts, and policy.

### 4. Bootstrap Missing Harness

If the target project does not contain `.harness/`, copy the closest template:

```text
templates/<project-type>/
```

Then adjust:

- Project name and description.
- Workspace type and repository list.
- Required commands.
- Artifact store.
- Git forbidden paths.

### 5. Switch To Target-Local Policy

After bootstrap, read and obey the target project's own:

- `AGENTS.md`
- `.harness/project-policy.yaml`
- `.harness/workspace-policy.yaml`
- `.harness/git-policy.yaml`
- `.harness/quality-policy.yaml`
- `.harness/architecture-policy.yaml`

Do not keep depending on this seed repository for day-to-day development.

## Minimum Target Harness

Every long-lived target project should have:

- `AGENTS.md`
- `.harness/project-policy.yaml`
- `.harness/workspace-policy.yaml`
- `.harness/git-policy.yaml`
- `.harness/quality-policy.yaml`
- `.harness/architecture-policy.yaml`
- `README.md`
- `ARCHITECTURE.md`
- `docs/GIT_POLICY.md`
- `scripts/harness-check`

Code projects should also provide `setup`, `lint`, `test`, and `dev` or `run`
entry points.
