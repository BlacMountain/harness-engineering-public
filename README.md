# Harness Seed Repository

这是一个 Harness Seed Repository，用于初始化下游项目的本地 harness。它不是普通文档仓库，也不要求 Agent 每次开发都回头依赖这里；正确用法是第一次初始化时从这里复制/生成目标项目自己的 `AGENTS.md`、`.harness/*.yaml`、`scripts/harness-check` 和 Git policy。

核心文档：

- [AGENTS.md](AGENTS.md)：本仓库 Agent 入口文件。
- [HARNESS_QUICKSTART.md](HARNESS_QUICKSTART.md)：Agent 初始化决策树。
- [templates/](templates)：按 project type 提供的最小 harness 模板。
- [docs/HARNESS_GUIDE.md](docs/HARNESS_GUIDE.md)：通用 harness 解释文档。
- [docs/GIT_POLICY.md](docs/GIT_POLICY.md)：Git、仓库边界和版本控制规则。
- [docs/PROJECT_TYPES.md](docs/PROJECT_TYPES.md)：项目类型、入库规则、artifact 和命令要求。
- [docs/DOCS_STRUCTURE.md](docs/DOCS_STRUCTURE.md)：文档分层、用途和边界。
- [docs/CODEX_CUSTOM_INSTRUCTIONS.md](docs/CODEX_CUSTOM_INSTRUCTIONS.md)：可直接放入 Codex 的自定义指令模板。
- [.harness/](.harness)：机器可读 policy source of truth。

## 常用命令

```bash
scripts/setup
scripts/lint
scripts/test
scripts/dev
scripts/harness-check
```

## 使用方式

1. 先阅读根目录 `AGENTS.md`，遵守启动协议。
2. 用 `HARNESS_QUICKSTART.md` 判断目标项目的 workspace type、repository 边界和 project type。
3. 如果目标项目缺少 `.harness/`，从 `templates/<project-type>/` 初始化最小 harness。
4. 初始化后切换到目标项目自己的 `AGENTS.md` 和 `.harness/*.yaml`。
5. 将反复出现的问题升级为目标项目本地的脚本、测试、lint 或 CI。

## 适用范围

本 seed 适用于需要让 Agent 可持续参与开发、测试、修复、审查和维护的软件项目。它不绑定具体语言或框架；若目标项目包含代码实现，应按项目技术栈补充独立虚拟环境、依赖管理和验证命令。
