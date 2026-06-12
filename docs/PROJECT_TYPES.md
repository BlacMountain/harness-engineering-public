# Project Types

Project type 决定目标项目的入库范围、依赖隔离方式、artifact 管理方式和最低命令入口。初始化目标项目时，必须先判断 project type，再选择 `templates/<project-type>/`。

## 类型矩阵

| 类型 | 入库内容 | 禁止入库 | 隔离环境 | Artifact 位置 | 必需命令 |
| --- | --- | --- | --- | --- | --- |
| `source-repo` | 源码、测试、文档、依赖清单、配置模板 | 虚拟环境、依赖目录、构建产物、覆盖率、数据、checkpoint、secret | 必需 | 可选，按项目需要 | setup、lint、test、dev、harness-check |
| `infra-repo` | Terraform、Ansible、Helm、Kubernetes、Compose、runbook | secret、credential、敏感 state、生成 plan | 通常不强制 | state backend、registry、secret manager | setup、lint、test、harness-check |
| `research-repo` | code、configs、docs、轻量 fixtures、复现实验脚本 | data、datasets、models、checkpoints、outputs、results、secret | 必需 | NAS、S3、Hugging Face、model registry | setup、lint、test、harness-check |
| `deployment-repo` | 部署脚本、manifest、环境模板、runbook | secret、release 包、生成 artifact、机器状态 | 通常不强制 | registry、release store | setup、lint、test、harness-check |
| `knowledge-repo` | 文档、policy、模板、验证脚本 | secret、本地环境文件、生成 artifact | 默认不强制 | local-unmanaged | lint、test、harness-check |

## `source-repo`

适用于产品代码、服务、CLI、库和应用。

典型结构：

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

入库：

- 源码。
- 测试。
- 文档。
- 依赖清单和 lock 文件。
- 配置模板。
- 测试所需的小型确定性 fixtures。

禁止入库：

- `.venv/`、`node_modules/`。
- `dist/`、`build/`、`coverage/`。
- `.env`、secret、credential。
- 大型生成 artifact。

## `infra-repo`

适用于 Terraform、Ansible、Helm、Kubernetes、Docker Compose 和系统基础设施。

典型结构：

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

入库：

- 基础设施代码。
- 不含 secret 的环境模板。
- Runbook。
- 验证脚本。

禁止入库：

- Secret 和 credential。
- 含敏感值的 Terraform state。
- 含敏感值的生成 plan。
- 机器本地配置。

Artifact 管理：

- Terraform remote state backend。
- Container registry。
- Secret manager。

## `research-repo`

适用于实验、模型训练、分析流水线和可复现实验。

典型结构：

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

入库：

- `code/`、`src/`、作为源码维护的 notebook。
- `configs/`。
- `docs/`。
- 轻量 fixtures。
- 复现实验脚本。

禁止入库：

- `data/`、`dataset/`、`datasets/`。
- `checkpoints/`。
- `models/`。
- `outputs/`、`results/`。
- 实验追踪缓存、大型日志、未经筛选的大型图表。

Artifact 管理：

- NAS。
- S3 或兼容对象存储。
- Hugging Face。
- Model registry。
- Experiment tracker。

## `deployment-repo`

适用于发布自动化、部署编排和环境模板。

典型结构：

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

入库：

- 部署脚本。
- Manifest。
- 不含 secret 的环境模板。
- Runbook 和 rollback 文档。

禁止入库：

- Secret 和 credential。
- 生成 release bundle。
- 机器特定状态。
- 生产环境本地 env 文件。

Artifact 管理：

- Release store。
- Container registry。
- Package registry。

## `knowledge-repo`

适用于文档、规范、prompt、runbook、policy 和模板。

典型结构：

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

入库：

- 文档。
- Policy 文件。
- 模板。
- 验证脚本。

禁止入库：

- Secret。
- 本地环境文件。
- 生成 artifact。
- 应保存在受控文档系统中的私有导出。

## 选择规则

- 可运行产品代码优先选择 `source-repo`。
- 数据、checkpoint、实验输出是一等问题时选择 `research-repo`。
- 仓库管理基础设施状态时选择 `infra-repo`。
- 仓库管理发布和部署流程时选择 `deployment-repo`。
- 仓库仅包含文档、policy 或模板时选择 `knowledge-repo`。
- 一个 workspace 包含多个不同类型仓库时，使用 `mono-workspace`，并让每个 repository 拥有自己的本地 harness。
