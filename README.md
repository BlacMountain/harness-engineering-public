# Harness Seed Repository

本仓库用于为目标项目初始化本地 harness。它提供启动协议、项目类型判断、可复制骨架、机器可读策略、文档规则和验证入口。目标项目完成初始化后，应以目标项目内的 `AGENTS.md`、`.harness/*.yaml` 和验证脚本作为长期约束源。

本仓库不作为目标项目的长期外部规则依赖。

## 功能边界

| 功能 | 入口 |
| --- | --- |
| Agent 启动协议 | `AGENTS.md` |
| 初始化决策树 | `HARNESS_QUICKSTART.md` |
| 可复制项目骨架 | `templates/<project-type>/` |
| 项目类型规则 | `docs/PROJECT_TYPES.md` |
| Git 与版本控制规则 | `docs/GIT_POLICY.md`、`.harness/git-policy.yaml` |
| 质量规则 | `docs/QUALITY.md`、`.harness/quality-policy.yaml` |
| 可靠性规则 | `docs/RELIABILITY.md` |
| 安全规则 | `docs/SECURITY.md` |
| 机器可读约束 | `.harness/*.yaml` |
| 通用建设原则 | `docs/HARNESS_GUIDE.md` |
| Codex 自定义指令 | `docs/CODEX_CUSTOM_INSTRUCTIONS.md` |

## 初始化流程

1. 读取 `AGENTS.md`，执行启动协议。
2. 使用 `HARNESS_QUICKSTART.md` 判断目标项目的 workspace type、repository boundary 和 project type。
3. 目标项目缺少 `.harness/` 时，从 `templates/<project-type>/` 初始化最小 harness。
4. 初始化后切换到目标项目本地 `AGENTS.md` 和 `.harness/*.yaml`。
5. 将长期规则沉淀到目标项目本地 policy、脚本、测试、lint 或 CI。

## 文档路由

| 写入内容 | 位置 | 说明 |
| --- | --- | --- |
| 系统应具备的能力、用户流程、验收标准 | `docs/product-specs/` | 先读 `docs/product-specs/README.md` |
| 当前任务的实施步骤、风险、验证方式 | `docs/exec-plans/active/` | 先读 `docs/exec-plans/README.md` |
| 已完成任务的结果和验证记录 | `docs/exec-plans/completed/` | 完成后从 `active` 迁移 |
| 剩余技术债 | `docs/exec-plans/tech-debt-tracker.md` | 记录可追踪的后续事项 |
| 长期架构或策略决策 | `docs/decisions/` | 先读 `docs/decisions/README.md` |
| 外部依据、协议、论文、平台说明 | `docs/references/` | 先读 `docs/references/README.md` |
| Git 长期规则 | `docs/GIT_POLICY.md`、`.harness/git-policy.yaml` | 文档解释规则，policy 承载机器约束 |
| 质量长期规则 | `docs/QUALITY.md`、`.harness/quality-policy.yaml` | 与验证命令保持一致 |
| 可靠性长期规则 | `docs/RELIABILITY.md` | 覆盖失败、恢复、观测要求 |
| 安全长期规则 | `docs/SECURITY.md` | 覆盖 secret、权限、输入和执行边界 |

目录内存在 `README.md` 时，以目录本地说明为准。根 `README.md` 只提供路由。

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
