# Harness Seed Repository

本仓库定义一套统一的 harness 工程化 seed。Agent 进入本仓库后，应学习这套 operating model，并把它迁移到目标项目的本地 `AGENTS.md`、`.harness/*.yaml`、docs 和验证命令中。

本仓库不是目标项目的长期外部规则依赖。目标项目初始化完成后，应以目标项目本地文件作为长期真相源。

## 先读什么

1. `AGENTS.md`：启动协议、强制检查、完成报告要求。
2. `HARNESS_QUICKSTART.md`：判断当前工作阶段，并选择下一步要创建或更新的文档/政策/脚本。
3. `docs/HARNESS_GUIDE.md`：理解 harness 的分层原则和迁移方式。
4. `.harness/*.yaml`：读取本 seed 仓库的机器可读约束。
5. `scripts/harness-check`：验证结构和 policy 是否一致。

## 四层模型

| 层级 | 路径 | 职责 |
| --- | --- | --- |
| 路由层 | `README.md`、`AGENTS.md`、`HARNESS_QUICKSTART.md` | 告诉 Agent 先读什么、当前任务下一步做什么 |
| 知识层 | `docs/`、`ARCHITECTURE.md` | 保存产品目标、执行计划、架构、决策、外部依据和治理说明 |
| 政策层 | `.harness/*.yaml` | 保存会被脚本稳定读取的 durable constraints |
| 执行层 | `scripts/`、lint、test、CI | 把长期规则机械化，给出可重复验证入口 |

## 文档落地路由

| 需要沉淀的内容 | 目标位置 | 使用说明 |
| --- | --- | --- |
| 系统应具备的能力、用户流程、验收标准 | `docs/product-specs/` | 先读 `docs/product-specs/README.md` |
| 当前任务的实施步骤、风险、验证方式 | `docs/exec-plans/active/` | 先读 `docs/exec-plans/README.md` |
| 已完成任务的结果和验证记录 | `docs/exec-plans/completed/` | 完成后从 `active` 迁移 |
| 剩余技术债 | `docs/exec-plans/tech-debt-tracker.md` | 记录可追踪的后续事项 |
| 长期架构或策略决策 | `docs/decisions/` | 先读 `docs/decisions/README.md` |
| 外部依据、协议、论文、平台说明 | `docs/references/` | 先读 `docs/references/README.md` |
| Git 和 artifact 规则 | `docs/GIT_POLICY.md`、`.harness/git-policy.yaml` | YAML 承载机器规则，文档解释人工判断 |
| 质量规则 | `docs/QUALITY.md`、`.harness/quality-policy.yaml` | 与验证命令保持一致 |
| 可靠性规则 | `docs/RELIABILITY.md` | 覆盖失败、恢复、观测要求 |
| 安全规则 | `docs/SECURITY.md` | 覆盖 secret、权限、输入和执行边界 |

目录内存在 `README.md` 时，以目录本地说明为准。根 `README.md` 只提供总路由。

## 初始化目标项目

1. 确认 workspace boundary 和 repository boundary。
2. 在目标项目创建或更新本地 `README.md`、`AGENTS.md`、`ARCHITECTURE.md`、`.harness/`、`docs/` 和统一命令入口。
3. 将目标项目的长期规则写入目标本地 `.harness/*.yaml`。
4. 将规则背景、人工判断和操作说明写入目标本地 docs。
5. 用目标项目自己的 `scripts/harness-check`、lint、test 或 CI 验证。
6. 初始化后切换到目标项目本地规则，不再依赖本 seed 仓库。

## 常用命令

```bash
scripts/setup
scripts/lint
scripts/test
scripts/dev
scripts/harness-check
```

## 适用范围

本 seed 适用于需要让 Agent 长期参与开发、测试、修复、审查和维护的软件项目。目标项目包含代码实现时，应按技术栈补充隔离依赖环境、依赖管理、运行命令和验证命令。
