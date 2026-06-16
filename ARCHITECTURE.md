# Architecture

本仓库维护一套面向 Agent 协作的软件工程 harness 方法。它由入口层、生命周期知识层和验证层组成。

## 系统边界

| 层级 | 路径 | 职责 |
| --- | --- | --- |
| 入口层 | `README.md`、`AGENTS.md`、`HARNESS_QUICKSTART.md` | 提供入口路由、Agent 行为控制和快速使用流程 |
| 生命周期知识层 | `ARCHITECTURE.md`、`docs/` | 保存 durable knowledge，并管理规格、计划、完成记录、技术债、决策、参考和治理规则 |
| 验证层 | `scripts/`、lint、test、CI | 机械化可检查规则，提供可重复验证入口 |

## 依赖方向

1. `README.md` 只做总入口和路由。
2. `AGENTS.md` 只约束 Agent 在本仓库中的行为。
3. `HARNESS_QUICKSTART.md` 承载快速使用流程。
4. `docs/` 是 durable knowledge 和项目周期管理的长期记录系统。
5. `ARCHITECTURE.md` 只记录结构不变量、职责边界和变更耦合。
6. `scripts/` 只执行可机械化检查，不承载长篇政策解释。

## 变更耦合

- 改变入口或阅读顺序时，同步更新 `README.md`、`AGENTS.md` 或 `HARNESS_QUICKSTART.md`。
- 改变结构层级、职责边界或依赖方向时，同步更新 `ARCHITECTURE.md` 和相关 ADR。
- 改变目标、行为或验收标准时，同步更新 `docs/product-specs/`。
- 改变任务执行状态时，同步更新 `docs/exec-plans/active/`、`completed/` 或 `tech-debt-tracker.md`。
- 改变 Git、质量、可靠性或安全规则时，同步更新对应治理文档和验证脚本。
- 不保留旧入口或兼容性说明，除非它们仍是当前架构的一部分。
