# Project Types

Project type decides what gets versioned, which commands are required, whether
an isolated environment is needed, and where artifacts belong.

## Summary Matrix

| Type | Git Source Of Truth | Not In Git | Isolated Env | Artifact Store | Required Commands |
| --- | --- | --- | --- | --- | --- |
| `source-repo` | source, tests, docs, manifests, config templates | envs, dependencies, build output, coverage, data, checkpoints, secrets | yes | optional | setup, lint, test, dev, harness-check |
| `infra-repo` | Terraform, Ansible, Helm, Kubernetes, Compose, runbooks | secrets, credentials, state with sensitive values, generated plans | usually no | state backend or registry | setup, lint, test, harness-check |
| `research-repo` | code, configs, docs, lightweight fixtures | data, datasets, checkpoints, models, outputs, results, secrets | yes | NAS, S3, Hugging Face, registry | setup, lint, test, harness-check |
| `deployment-repo` | deployment scripts, manifests, env templates, runbooks | secrets, releases, generated artifacts, machine state | usually no | registry or release store | setup, lint, test, harness-check |
| `knowledge-repo` | docs, policy, templates, validation scripts | secrets, local envs, generated artifacts | no by default | local-unmanaged | lint, test, harness-check |

## `source-repo`

Use for product code, services, CLIs, libraries, and applications.

Typical structure:

```text
.
├── AGENTS.md
├── README.md
├── ARCHITECTURE.md
├── .harness/
├── docs/
├── scripts/
├── src/ or app/
└── tests/
```

Git:

- Source code.
- Tests.
- Documentation.
- Dependency manifests and lock files.
- Configuration templates.
- Small deterministic fixtures required by tests.

Not Git:

- `.venv/`, `node_modules/`
- `dist/`, `build/`, `coverage/`
- `.env`, secrets, credentials
- Large generated artifacts

## `infra-repo`

Use for Terraform, Ansible, Helm, Kubernetes, Docker Compose, and system
infrastructure.

Typical structure:

```text
.
├── AGENTS.md
├── README.md
├── ARCHITECTURE.md
├── .harness/
├── docs/
├── scripts/
├── terraform/ or infra/
├── ansible/
└── helm/ or k8s/
```

Git:

- Infrastructure code.
- Environment templates without secrets.
- Runbooks.
- Validation scripts.

Not Git:

- Secrets and credentials.
- Terraform state containing sensitive values.
- Generated plans containing sensitive values.
- Machine-local config.

Artifact/store:

- Terraform remote state backend.
- Container registry.
- Secret manager.

## `research-repo`

Use for experiments, model training, analysis pipelines, and reproducible
research.

Typical structure:

```text
.
├── AGENTS.md
├── README.md
├── ARCHITECTURE.md
├── .harness/
├── configs/
├── docs/
├── scripts/
├── src/ or notebooks/
└── tests/
```

Git:

- `code/`, `src/`, notebooks intended as source.
- `configs/`.
- `docs/`.
- Lightweight fixtures.
- Reproduction scripts.

Not Git:

- `data/`, `dataset/`, `datasets/`
- `checkpoints/`
- `models/`
- `outputs/`, `results/`
- Experiment tracker cache, large logs, generated plots unless intentionally
  curated and small.

Artifact/store:

- NAS.
- S3 or compatible object storage.
- Hugging Face.
- Model registry.
- Experiment tracker.

## `deployment-repo`

Use for release automation, deployment orchestration, and environment templates.

Typical structure:

```text
.
├── AGENTS.md
├── README.md
├── ARCHITECTURE.md
├── .harness/
├── docs/
├── scripts/
├── manifests/
└── environments/
```

Git:

- Deployment scripts.
- Manifests.
- Environment templates without secrets.
- Runbooks and rollback docs.

Not Git:

- Secrets and credentials.
- Generated release bundles.
- Machine-specific state.
- Production-only local env files.

Artifact/store:

- Release store.
- Container registry.
- Package registry.

## `knowledge-repo`

Use for docs, standards, prompts, runbooks, policy, and templates.

Typical structure:

```text
.
├── AGENTS.md
├── README.md
├── ARCHITECTURE.md
├── .harness/
├── docs/
├── templates/
└── scripts/
```

Git:

- Documentation.
- Policy files.
- Templates.
- Validation scripts.

Not Git:

- Secrets.
- Local env files.
- Generated artifacts.
- Private exports that should live in a managed document system.

## Choosing A Type

When uncertain:

- Prefer `source-repo` for runnable product code.
- Prefer `research-repo` when data, checkpoints, or experiment outputs are
  first-class concerns.
- Prefer `infra-repo` when the repository manages infrastructure state.
- Prefer `deployment-repo` when it deploys or releases already-built systems.
- Prefer `knowledge-repo` when it contains policy or docs and no runtime.

If a workspace contains several of these, use `mono-workspace` and give each
repository its own target-local harness.
