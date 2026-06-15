# Architecture

本仓库是 Harness Seed Repository。它不提供业务运行时，不作为目标项目的长期外部依赖。它的职责是在目标项目初始化阶段提供本地 harness 骨架，并把长期约束迁移到目标项目自身。

## 系统边界

| 层级 | 路径 | 职责 |
| --- | --- | --- |
| 启动协议 | `AGENTS.md` | 定义 Agent 在目标项目开始代码工作前必须执行的步骤 |
| 初始化决策 | `HARNESS_QUICKSTART.md` | 判断 workspace boundary、repository boundary 和 project type |
| 机器策略 | `.harness/*.yaml` | 保存本 seed 仓库的可检查约束 |
| 可复制骨架 | `templates/<project-type>/` | 为目标项目生成本地 `AGENTS.md`、`.harness/`、docs 和验证入口 |
| 人类说明 | `README.md`、`docs/` | 解释功能边界、目标项目文档落地路由、项目类型和治理规则 |
| 验证入口 | `scripts/` | 检查 seed 仓库结构、模板结构和基础约束 |

## 依赖方向

1. `AGENTS.md` 只路由到 quickstart、templates 和目标项目本地 policy。
2. `HARNESS_QUICKSTART.md` 只负责初始化判断，不承载长期规则。
3. `.harness/*.yaml` 是本仓库 durable constraints 的机器可读来源。
4. `docs/*.md` 解释规则和使用场景，不得与 `.harness/*.yaml` 冲突。
5. `scripts/harness-check` 将必须长期保持的结构规则机械化。
6. `templates/<project-type>/` 复制到目标项目后，目标项目本地文件成为长期约束源。

## 目录职责

- `.harness/`：定义 seed 仓库自身的 project、workspace、git、quality 和 architecture policy。
- `templates/`：按 project type 提供最小可复制 harness；模板重整应保持目标项目可独立运行。
- `docs/product-specs/`：记录本 seed 仓库应提供的能力和验收标准。
- `docs/exec-plans/`：记录复杂变更的执行计划、完成记录和技术债。
- `docs/decisions/`：记录长期架构或策略决策。
- `docs/references/`：记录外部依据和本地摘要。
- `docs/GIT_POLICY.md`、`docs/QUALITY.md`、`docs/RELIABILITY.md`、`docs/SECURITY.md`：解释长期治理规则。
- `scripts/`：提供 setup、lint、test、dev 和 harness-check 入口。

## 规则沉淀顺序

长期规则必须按以下顺序沉淀：

1. `.harness/*.yaml`
2. `scripts/harness-check`、lint、测试或 CI
3. `docs/*.md`
4. `AGENTS.md`，仅限启动阶段必须看到的规则

## 变更要求

- 修改目录结构时，同步更新 `.harness/project-policy.yaml` 和 `scripts/harness-check`。
- 修改目标项目文档落地路由时，同步更新根 `README.md` 和相关目录本地 `README.md`。
- 修改模板结构时，验证所有 `templates/<project-type>/scripts/harness-check`。
- 不保留旧入口或兼容性说明，除非它们仍是当前架构的一部分。
