# Product Specs Index

本 seed 仓库的产品目标是提供一套可迁移到目标项目的 harness operating model，并保留 `Harness Seed Repository` 作为工程名称。

## 当前能力

- 通过 `README.md`、`AGENTS.md` 和 `HARNESS_QUICKSTART.md` 提供短入口和任务阶段路由。
- 通过 `docs/` 保存产品目标、执行计划、决策、外部依据和治理说明。
- 通过 `.harness/policy.yaml` 保存会被脚本读取的机器可读约束。
- 通过 `scripts/harness-check` 验证 seed 仓库结构和 policy。
- 通过 `scripts/setup`、`scripts/lint`、`scripts/test`、`scripts/dev` 提供统一命令入口。

## 非目标

- 不作为目标项目的长期外部规则依赖。
- 不替代目标项目本地 `AGENTS.md`、`.harness/policy.yaml`、docs 和验证脚本。
- 不在 seed 仓库中定义具体业务项目的运行时实现。

## 验收标准

- 新 Agent 能从根 `README.md` 和 `AGENTS.md` 找到启动流程。
- `HARNESS_QUICKSTART.md` 能把常见任务导向 spec、plan、ADR、reference、policy 或 check。
- `.harness/policy.yaml` 被 `scripts/harness-check` 消费，而不是成为第二套说明书。
- 目标项目文档落地路由能从根 `README.md` 找到对应目录和 policy 文档。
