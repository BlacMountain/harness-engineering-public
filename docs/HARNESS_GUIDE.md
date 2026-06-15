# 通用 Harness 指导文档

本文定义本仓库采用的 harness engineering 方法。这里的 harness 指支撑 AI Agent 完成软件工程任务的一组仓库结构、文档、机器政策、脚本、验证和反馈回路。

本仓库的定位是 Harness Seed Repository：它提供一套可迁移到目标项目的 operating model。目标项目初始化完成后，应依靠目标项目自己的 `AGENTS.md`、`.harness/*.yaml`、docs 和验证命令持续约束 Agent。

官方参考：[OpenAI Harness Engineering](https://openai.com/zh-Hans-CN/index/harness-engineering/)

## 1. 目标

Harness 的目标不是让 Agent 只靠提示词工作，而是让它在仓库内拥有足够清晰、可执行、可验证的环境，能够稳定完成以下工作：

- 理解项目边界和业务目标。
- 找到正确上下文，而不是依赖一次性提示。
- 独立启动、测试、复现和验证变更。
- 遵守架构、质量、安全和可靠性约束。
- 将失败、反馈和经验沉淀回仓库。

一句话原则：人类定义目标和边界，Agent 执行、验证并把经验编码进系统。

## 2. 四层模型

### 路由层

路由层包含 `README.md`、`AGENTS.md` 和 `HARNESS_QUICKSTART.md`。它只回答：

- 先读什么。
- 当前任务下一步去哪里。
- 哪些动作必须执行。
- 何时停止并向用户确认。

`AGENTS.md` 是 Agent 入口地图，不是百科全书。它必须短、稳定、可维护。

### 知识层

知识层包含 `docs/` 和 `ARCHITECTURE.md`。它保存项目长期事实：

- `docs/product-specs/`：系统应该是什么。
- `docs/exec-plans/`：这次任务怎么做。
- `docs/decisions/`：为什么长期这样选。
- `docs/references/`：外部依据是什么。
- `docs/GIT_POLICY.md`、`docs/QUALITY.md`、`docs/RELIABILITY.md`、`docs/SECURITY.md`：横切治理规则。

目录内存在 `README.md` 时，以目录本地说明为准。总路由只放在根 `README.md`。

### 政策层

政策层包含 `.harness/*.yaml`。它不是第二套 prose docs，而是机器可读 contract。

适合放入 `.harness/` 的内容：

- 必需文件和目录。
- 必需命令入口。
- Git 禁止路径。
- workspace 和 repository 边界。
- 质量检查和完成报告要求。
- 可被脚本稳定解析的架构约束。

不适合放入 `.harness/` 的内容：

- 产品说明。
- 执行计划正文。
- 决策理由。
- 外部资料摘要。
- 人类风格偏好。

如果某条规则不会被脚本、lint、测试、hook、CI 或 Agent 稳定读取，就优先写入 docs，而不是伪装成 policy。

### 执行层

执行层包含 `scripts/`、lint、test、hook 和 CI。它把长期规则从文档提升为可重复验证。

最低入口：

```bash
scripts/setup
scripts/lint
scripts/test
scripts/dev
scripts/harness-check
```

如果某个入口不适用于当前项目，命令应输出清晰说明，而不是静默成功。

## 3. 目标项目迁移

初始化或修复目标项目时：

1. 确认 workspace boundary 和 repository boundary。
2. 创建或更新目标本地 `README.md`、`AGENTS.md`、`ARCHITECTURE.md`、`.harness/`、docs 和验证命令。
3. 将 durable constraints 写入目标本地 `.harness/*.yaml`。
4. 将背景、判断、操作说明写入目标本地 docs。
5. 将可机械化规则写入脚本、lint、测试、hook 或 CI。
6. 初始化后切换到目标本地规则。

目标项目不应长期依赖本 seed 仓库作为外部规则源。

## 4. 文档治理

文档需要像代码一样治理：

- 每个长期文档必须有明确用途。
- 每个目录应有索引或 README。
- 决策文档应记录状态、背景、选择、替代方案和后果。
- 过期文档应删除、归档或标记。
- 文档和代码冲突时，应修正文档或代码，不保留模糊状态。
- 未决事项应进入 `docs/exec-plans/tech-debt-tracker.md` 或执行计划，而不是只留在聊天记录。

## 5. Agent 工作流

推荐标准流程：

1. 读取 `AGENTS.md`、`HARNESS_QUICKSTART.md` 和 `.harness/*.yaml`。
2. 判断 workspace 和 repository 边界。
3. 运行 `git status --short`。
4. 定位相关代码、测试、规格和 policy。
5. 明确目标、非目标和验收标准。
6. 做最小可行变更。
7. 运行本地验证。
8. 若失败，读取日志并修复。
9. 更新相关 policy、文档、脚本或测试。
10. 汇报变更、验证结果、git status 和剩余风险。

## 6. 反模式

避免以下做法：

- 只写长 prompt，不改仓库结构。
- 把所有规则塞进一个巨大 `AGENTS.md`。
- 只有软提示，没有可执行检查。
- 让 Agent 依赖外部聊天记录。
- 没有测试就让 Agent 大规模改代码。
- 文档过期但没有检查机制。
- 只靠人工 review 维护架构。
- 对反复问题只提醒，不编码成规则。
- 同时维护多套相互竞争的入口和模板叙事。

## 7. 判断标准

一个 harness 是否合格，可以用以下问题判断：

- 新 Agent 是否能在 5 分钟内知道项目如何运行和验证。
- Agent 是否能独立找到相关规格、架构和测试。
- 失败时是否有足够日志供 Agent 继续推进。
- 关键规则是否被工具强制执行。
- 反复出现的问题是否被编码成规则。
- 文档是否随着代码变化同步更新。
- 人类是否主要在判断目标和边界，而不是反复解释环境。
