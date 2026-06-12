# Git Policy

Git policy defines what belongs in version control, where git operations may
run, and how Agent changes stay reviewable.

## 使用方式

什么时候读：

- 初始化或判断 repository 边界时。
- 修改 `.gitignore`、artifact 目录、数据目录或生成物路径时。
- 准备 stage、commit、push 或创建 PR 前。

什么时候更新：

- 新增 project type、artifact store 或生成目录时。
- 发现 Agent 反复误提交某类文件时。
- `.harness/git-policy.yaml` 或 `.gitignore` 的规则发生变化时。

关系：

- `.harness/git-policy.yaml` 是机器可读规则。
- `.gitignore` 是本地执行边界。
- 本文档解释规则原因、使用场景和人工判断。

## Repository Types

- `source-repo`: product, service, CLI, library, or application source.
- `infra-repo`: Terraform, Ansible, Helm, Kubernetes, Docker Compose, or system
  infrastructure.
- `research-repo`: experiments, training code, reproducible analysis, and
  model development.
- `deployment-repo`: deployment orchestration, release scripts, and environment
  configuration templates.
- `knowledge-repo`: documentation, policy, guidelines, and reusable operational
  knowledge.

## Workspace vs Repository

A workspace may contain one or more repositories. A repository is the git root
that owns source history.

MUST:

- Read `.harness/workspace-policy.yaml` before git initialization.
- Run `git status --short` before modifying an existing repository.
- Confirm the target repository path before staging, committing, or pushing.

NEVER:

- Assume `workspace/` is a git repository.
- Run `git init .` at a workspace root unless policy says `single-repo`.
- Stage files outside the target repository.

## What Goes Into Git

Source repositories SHOULD version:

- Source code.
- Tests.
- Documentation.
- Configuration templates.
- Dependency manifests and lock files.
- Small deterministic fixtures needed by tests.

Infrastructure and deployment repositories SHOULD version:

- Terraform, Helm, Kubernetes, Ansible, Docker Compose, and deployment scripts.
- Environment templates without secrets.
- Runbooks and operational documentation.

Research repositories SHOULD version:

- Experiment code.
- Configurations.
- Lightweight fixtures.
- Documentation describing data sources, artifact locations, and reproduction
  steps.

Knowledge repositories SHOULD version:

- Markdown documents.
- Machine-readable policy files.
- Validation scripts.
- Templates and examples.

## What Must Not Go Into Git

NEVER commit:

- `.venv/`, `venv/`, `env/`
- `node_modules/`
- `dist/`, `build/`, `coverage/`
- `data/`, `dataset/`, `datasets/`
- `models/`, `checkpoints/`
- `outputs/`, `results/`, `artifacts/`
- `.env`, `.env.*`, secrets, tokens, credentials, private keys
- `.DS_Store`

Files larger than 100 MB MUST NOT be committed. Put large artifacts in an
artifact store such as S3, Hugging Face, a registry, or a managed local NAS, and
document the location.

## Commit Rules

Use small, purposeful commits. Commit types:

- `feat`: user-visible capability.
- `fix`: bug fix.
- `refactor`: behavior-preserving structure change.
- `docs`: documentation-only change.
- `test`: test-only change.
- `chore`: tooling, policy, or maintenance change.

Each commit SHOULD have:

- A clear scope.
- Verification command output available in the task report.
- No unrelated rewrites.

## Agent Rules

MUST:

- Preserve user changes.
- Keep generated changes reviewable.
- Report files changed, commands run, and current git status.
- Add or update `.gitignore` when introducing generated outputs.

NEVER:

- Use destructive git commands to clean up without explicit user request.
- Revert user edits to make local checks pass.
- Commit files that policy forbids.
