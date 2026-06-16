# Product Specs Index

本 seed 仓库的产品目标是提供一套可迁移到目标项目的 harness operating model，并保留 `Harness Seed Repository` 作为工程名称。

## 当前能力

- 通过 `README.md`、`AGENTS.md` 和 `HARNESS_QUICKSTART.md` 提供短入口和任务阶段路由。
- 通过 `docs/` 保存 durable knowledge，并管理规格、执行计划、完成记录、技术债、决策、参考和治理说明。
- 通过 `scripts/lint`、`scripts/test` 和 CI 验证 seed 仓库结构和规则。
- 通过 `scripts/setup`、`scripts/lint`、`scripts/test`、`scripts/dev` 提供统一命令入口。

## 非目标

- 不作为目标项目的长期外部规则依赖。
- 不替代目标项目本地 `AGENTS.md`、docs 和验证脚本。
- 不在 seed 仓库中定义具体业务项目的运行时实现。

## 验收标准

- 新 Agent 能从根 `README.md` 和 `AGENTS.md` 找到启动流程。
- `HARNESS_QUICKSTART.md` 能把常见任务导向 spec、plan、ADR、reference、governance doc 或 validation script。
- `docs/exec-plans/active/`、`completed/` 和 `tech-debt-tracker.md` 明确承担项目周期管理职责。
- 目标项目文档落地路由能从 `HARNESS_QUICKSTART.md` 找到对应目录和治理文档。
