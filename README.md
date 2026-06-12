# Harness Engineering Guide

这是一个通用 harness 指导文档仓库，用于沉淀面向 AI Agent / Codex 的工程环境、约束、反馈回路和文档规范。

核心文档：

- [docs/HARNESS_GUIDE.md](docs/HARNESS_GUIDE.md)：通用 harness 指南。
- [docs/CODEX_CUSTOM_INSTRUCTIONS.md](docs/CODEX_CUSTOM_INSTRUCTIONS.md)：可直接放入 Codex 的自定义指令模板。
- [AGENTS.md](AGENTS.md)：本仓库唯一 Agent 入口文件。
- [docs/GIT_POLICY.md](docs/GIT_POLICY.md)：Git、仓库边界和版本控制规则。
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

1. 先阅读 `docs/HARNESS_GUIDE.md`，确认当前项目所处成熟度阶段。
2. 读取 `.harness/*.yaml`，确认 project type、workspace type、Git policy 和质量要求。
3. 按“最低可用 Harness”建立目录、命令、测试、日志和文档入口。
4. 将反复出现的问题升级为可执行约束，例如 lint、测试、CI 或脚本。
5. 定期回收过期文档、漂移架构和重复实现。

## 适用范围

本指南适用于需要让 Agent 可持续参与开发、测试、修复、审查和维护的软件项目。它不绑定具体语言或框架；若项目包含代码实现，应按项目技术栈补充独立虚拟环境、依赖管理和验证命令。
