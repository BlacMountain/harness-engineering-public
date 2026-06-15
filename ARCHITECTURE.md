# Architecture

本仓库是 Harness Seed Repository。它的职责是定义一套可迁移到目标项目的 harness operating model：入口、知识层、机器政策层和执行层。目标项目初始化完成后，目标项目本地文件成为长期真相源。

## 系统边界

| 层级 | 路径 | 职责 |
| --- | --- | --- |
| 路由层 | `README.md`、`AGENTS.md`、`HARNESS_QUICKSTART.md` | 提供阅读顺序、任务阶段判断和强制工作流 |
| 知识层 | `docs/`、`ARCHITECTURE.md` | 保存产品目标、执行计划、决策、外部依据和治理说明 |
| 政策层 | `.harness/*.yaml` | 保存本 seed 仓库可被脚本读取的 durable constraints |
| 执行层 | `scripts/` | 校验结构、policy、入口命令和版本控制边界 |

## 依赖方向

1. `AGENTS.md` 只保留启动阶段必须看到的短规则。
2. `HARNESS_QUICKSTART.md` 只负责阶段判断和下一工件路由。
3. `.harness/*.yaml` 是机器可读政策层，不复制长篇说明。
4. `docs/*.md` 解释规则背景、人工判断和长期知识。
5. `scripts/harness-check` 必须真实消费 `.harness/*.yaml`。
6. 目标项目初始化后，目标项目本地 harness 成为长期约束源。

## 目录职责

- `.harness/`：定义本 seed 仓库的 project、workspace、git、quality 和 architecture policy。
- `docs/product-specs/`：记录本 seed 仓库应提供的能力和验收标准。
- `docs/exec-plans/`：记录复杂变更的执行计划、完成记录和技术债。
- `docs/decisions/`：记录长期架构或策略决策。
- `docs/references/`：记录外部依据和本地摘要。
- `docs/GIT_POLICY.md`、`docs/QUALITY.md`、`docs/RELIABILITY.md`、`docs/SECURITY.md`：解释长期治理规则。
- `scripts/`：提供 setup、lint、test、dev 和 harness-check 入口。

## 规则沉淀顺序

长期规则应按以下顺序沉淀：

1. `.harness/*.yaml`
2. `scripts/harness-check`、lint、测试或 CI
3. `docs/*.md`
4. `AGENTS.md`，仅限启动阶段必须看到的规则

## 变更要求

- 修改目录结构时，同步更新 `.harness/project-policy.yaml` 和 `scripts/harness-check`。
- 修改目标项目文档落地路由时，同步更新根 `README.md` 和相关目录本地 `README.md`。
- 修改 policy 语义时，同步更新 `.harness/*.yaml`、解释性文档和验证脚本。
- 不保留旧入口或兼容性说明，除非它们仍是当前架构的一部分。
