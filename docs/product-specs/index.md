# Product Specs Index

本 seed 仓库的产品目标是提供可复制的本地 harness 初始化能力。

## 当前能力

- 通过 `AGENTS.md` 定义 Agent 启动协议。
- 通过 `HARNESS_QUICKSTART.md` 判断 workspace boundary、repository boundary 和 project type。
- 通过 `templates/<project-type>/` 初始化目标项目的本地 harness。
- 通过 `.harness/*.yaml` 保存本 seed 仓库的机器可读约束。
- 通过 `scripts/harness-check` 验证 seed 仓库和模板结构。
- 通过 `docs/GIT_POLICY.md`、`docs/QUALITY.md`、`docs/RELIABILITY.md`、`docs/SECURITY.md` 解释治理规则。

## 非目标

- 不作为目标项目的长期外部规则依赖。
- 不替代目标项目本地 `AGENTS.md`、`.harness/*.yaml` 和验证脚本。
- 不在 seed 仓库中定义具体业务项目的运行时实现。

## 验收标准

- 新 Agent 能从根 `AGENTS.md` 找到初始化流程。
- 目标项目可从 `templates/<project-type>/` 获得最小 harness。
- `scripts/harness-check` 能检查 seed 仓库必需结构。
- 文档路由能从根 `README.md` 找到对应目录和 policy 文档。
