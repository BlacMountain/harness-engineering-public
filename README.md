# Harness Seed Repository

本仓库定义一套面向 Agent 协作的软件工程 harness 方法，用于说明如何把项目说明、Agent 行为约束、生命周期文档和验证脚本组织成一个可读、可运行、可验证、可迭代的工程系统。

## 先读什么

1. `AGENTS.md`：Agent 在本仓库工作的启动协议、硬规则和交付要求。
2. `HARNESS_QUICKSTART.md`：快速判断当前任务阶段，并进入 harness 使用流程。
3. `ARCHITECTURE.md`：本仓库的结构不变量、职责边界和依赖方向。
4. 任务相关的 `docs/` 文档：规格、执行计划、决策、参考和治理规则。
5. `scripts/lint`、`scripts/test`：本仓库的基础验证入口。

## 三层模型

| 层级 | 路径 | 职责 |
| --- | --- | --- |
| 入口层 | `README.md`、`AGENTS.md`、`HARNESS_QUICKSTART.md` | 提供仓库入口、Agent 行为控制和快速使用流程 |
| 生命周期知识层 | `ARCHITECTURE.md`、`docs/` | 保存 durable knowledge，并管理项目周期：规格、计划、完成记录、技术债、决策、参考和治理规则 |
| 验证层 | `scripts/`、lint、test、CI | 将可机械化的规则落实为可重复检查 |

## docs 的职责

`docs/` 负责开发生命周期知识和项目周期记录：

| 内容 | 位置 |
| --- | --- |
| 目标、范围、行为和验收标准 | `docs/product-specs/` |
| 整理前的草稿需求和外部讨论材料 | `docs/exec-plans/prepare/` |
| 当前任务的执行计划 | `docs/exec-plans/active/` |
| 已完成任务的结果和验证记录 | `docs/exec-plans/completed/` |
| 后续事项和技术债 | `docs/exec-plans/tech-debt-tracker.md` |
| 长期架构、治理或策略决策 | `docs/decisions/` |
| 外部资料、协议、论文或平台依据 | `docs/references/` |
| Git、质量、可靠性、安全治理规则 | `docs/GIT_POLICY.md`、`docs/QUALITY.md`、`docs/RELIABILITY.md`、`docs/SECURITY.md` |

目标项目交付文档服务目标项目的用户、部署者或运维者，例如 API 文档、用户手册、部署说明、SDK 文档、operator docs 和产品说明。它们由目标项目团队在约定的交付文档位置管理。

目录内存在 `README.md` 时，以目录本地说明为准。根 `README.md` 只保留总入口和路由。

## 使用流程

快速使用流程见 `HARNESS_QUICKSTART.md`。

## 常用命令

```bash
scripts/setup
scripts/lint
scripts/test
scripts/dev
```
