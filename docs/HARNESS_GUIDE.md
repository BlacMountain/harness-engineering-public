# 通用 Harness 指导文档

本文基于 OpenAI《工程技术：在智能体优先的世界中利用 Codex》的工程经验整理，目标是定义一套可复用的 harness 建设方法。这里的 harness 指支撑 AI Agent 完成软件工程任务的一组仓库结构、文档、脚本、约束、观测和反馈回路。

本仓库的定位是 Harness Seed Repository：它负责初始化目标项目的本地 harness；目标项目初始化完成后，应依靠自己的 `AGENTS.md`、`.harness/*.yaml` 和 `scripts/harness-check` 持续约束 Agent。

官方参考：[OpenAI Harness Engineering](https://openai.com/zh-Hans-CN/index/harness-engineering/)

## 1. 目标

Harness 的目标不是让 Agent “更听话”，而是让它在仓库内拥有足够清晰、可执行、可验证的环境，能够稳定完成以下工作：

- 理解项目边界和业务目标。
- 找到正确上下文，而不是依赖一次性提示。
- 独立启动、测试、复现和验证变更。
- 遵守架构、质量、安全和可靠性约束。
- 将失败、反馈和经验沉淀回仓库。

一句话原则：人类定义目标和边界，Agent 执行、验证并把经验编码进系统。

## 2. 核心原则

### 2.1 仓库是记录系统

Agent 看不到的上下文，在执行时等同于不存在。产品决策、架构约束、运行方式、验收标准、常见问题和技术债都应进入仓库，并随代码一起版本化。

推荐做法：

- 用短文档作为入口，用目录结构承载细节。
- 把产品规格、架构规则、执行计划、质量标准分开放置。
- 所有长期有效规则都应进入文档、脚本、测试、lint 或 CI。
- 避免把关键约束只留在聊天、会议记录或个人记忆中。

### 2.2 AGENT 入口是地图，不是百科全书

`AGENTS.md` 是唯一 Agent 入口文件。它应简短、稳定、可维护，负责告诉 Agent 去哪里找信息，而不是复制全部信息。

建议入口文件包含：

- 项目目标。
- 常用命令。
- 目录地图。
- 必守约束。
- 验证要求。
- 遇到歧义时的处理方式。

不建议包含：

- 大段历史背景。
- 过期决策。
- 大量框架教程。
- 无法机械验证的主观偏好。

### 2.3 渐进式披露

Agent 初始上下文有限，应先看到稳定入口，再按任务需要深入读取相关文档。

推荐信息层级：

1. `AGENTS.md`：Agent 入口地图和硬性工作流。
2. `.harness/*.yaml`：机器可读 policy source of truth。
3. `README.md`：项目用途和启动方式。
4. `ARCHITECTURE.md`：系统边界和依赖方向。
5. `docs/GIT_POLICY.md`：Git、仓库边界和版本控制规则。
6. `docs/product-specs/`：产品和用户流程。
7. `docs/exec-plans/`：复杂任务计划、进度和决策记录。
8. `docs/references/`：外部协议、SDK、模型、平台参考。
9. `docs/QUALITY.md`、`RELIABILITY.md`、`SECURITY.md`：质量、安全和可靠性规则。

更详细的项目类型规则见 `docs/PROJECT_TYPES.md`。目标项目文档落地路由见根 `README.md`，具体目录写法见各目录本地 `README.md`。

### 2.4 约束要机械化

文档只能降低误解，不能防止漂移。长期规则必须尽量变成可执行检查。

常见机械化手段：

- 格式化：统一代码风格。
- Lint：禁止危险模式和架构违规。
- 类型检查：提前暴露接口不一致。
- 单元测试：保护局部行为。
- 集成测试：保护跨模块协作。
- 结构测试：验证目录、依赖方向和命名约定。
- CI：把关键验证放进合并流程。
- 文档检查：验证链接、目录、索引和新鲜度。

### 2.5 反馈回路优先

Agent 能否高质量工作，取决于它是否能看到执行结果。

最低反馈回路：

- 一键安装依赖。
- 一键运行测试。
- 一键启动本地服务。
- 明确日志位置。
- 明确失败排查路径。

增强反馈回路：

- UI 自动化与截图。
- 结构化日志。
- 指标和 tracing。
- 复现脚本。
- 性能基准。
- 回归测试生成流程。

### 2.6 人类品味要编码进系统

如果某类问题反复通过人工 review 才发现，说明 harness 还缺一条规则。优先把它沉淀为：

- 文档规则。
- 模板。
- lint。
- 测试。
- CI 检查。
- 自动修复脚本。

### 2.7 Harness Policy Layer

仅靠 `AGENTS.md`、README 或自定义指令，Agent 容易在长上下文中漏掉规则。长期约束必须进入 `.harness/`，成为机器可读的 policy source of truth。

最低 policy layer：

- `.harness/project-policy.yaml`：project type、必需文件、必需命令、环境隔离、文档结构。
- `.harness/workspace-policy.yaml`：workspace type、repository 边界和 git 初始化策略。
- `.harness/git-policy.yaml`：什么进 git、什么不进 git、大文件规则、提交规则。
- `.harness/quality-policy.yaml`：setup、lint、test、dev 和验证汇报要求。
- `.harness/architecture-policy.yaml`：目录边界、依赖方向和架构规则。

规则分层：

- `AGENTS.md` MUST 只保留任务启动时必须看到的短规则。
- `.harness/*.yaml` MUST 保存 durable constraints。
- `docs/*.md` SHOULD 解释规则的背景、场景和人工判断依据。
- `scripts/`、lint、测试或 CI SHOULD 强制执行可机械化规则。

### 2.7.1 Seed 与 Target 的职责

参考工程负责 bootstrap，目标工程负责持续约束。

- Seed repository MUST 提供 `AGENTS.md`、`HARNESS_QUICKSTART.md` 和 `templates/<project-type>/`。
- Seed repository SHOULD 提供解释性 `docs/`，但 docs 不应成为目标项目启动时的唯一依赖。
- Target project MUST 拥有自己的 `AGENTS.md`、`.harness/*.yaml` 和验证命令。
- Agent MUST 在目标项目 harness 初始化后切换到目标本地规则，不继续依赖 seed 仓库。

### 2.8 Project Type

新项目初始化前 MUST 判断 project type，因为不同项目的 git 边界、依赖环境和 artifact 规则不同。

- `source-repo`：产品、服务、CLI、库或应用源码。
- `infra-repo`：Terraform、Ansible、Helm、Kubernetes、Docker Compose 或系统基础设施。
- `research-repo`：实验代码、训练配置、论文复现、模型开发。
- `deployment-repo`：部署编排、发布脚本、环境配置模板。
- `knowledge-repo`：文档、规范、知识库和 reusable policy。

研究项目默认只把 `code/`、`configs/`、`docs/` 和轻量 fixtures 放入 git。`data/`、`datasets/`、`models/`、`checkpoints/`、`outputs/`、`results/` MUST NOT 进入 git。

### 2.9 Workspace Type

新项目初始化前 MUST 判断 workspace type。

- `single-repo`：当前目录就是一个 git repository。
- `mono-repo`：一个 git repository 内含多个 package 或 service。
- `mono-workspace`：一个 workspace 下有多个独立 git repository。

Agent NEVER 在 workspace 根目录默认执行 `git init .`。如果 workspace/repository 边界不清晰，MUST 先向用户反馈。

## 3. 推荐仓库结构

```text
.
├── README.md
├── AGENTS.md
├── HARNESS_QUICKSTART.md
├── ARCHITECTURE.md
├── .harness/
│   ├── project-policy.yaml
│   ├── workspace-policy.yaml
│   ├── git-policy.yaml
│   ├── quality-policy.yaml
│   └── architecture-policy.yaml
├── Makefile
├── templates/
│   ├── source-repo/
│   ├── infra-repo/
│   ├── research-repo/
│   ├── deployment-repo/
│   └── knowledge-repo/
├── docs/
│   ├── HARNESS_GUIDE.md
│   ├── GIT_POLICY.md
│   ├── PROJECT_TYPES.md
│   ├── product-specs/
│   │   ├── README.md
│   │   └── index.md
│   ├── exec-plans/
│   │   ├── README.md
│   │   ├── active/
│   │   ├── completed/
│   │   └── tech-debt-tracker.md
│   ├── decisions/
│   │   ├── README.md
│   │   └── index.md
│   ├── references/
│   │   ├── README.md
│   │   └── index.md
│   ├── QUALITY.md
│   ├── RELIABILITY.md
│   └── SECURITY.md
├── scripts/
│   ├── setup
│   ├── test
│   ├── lint
│   ├── dev
│   └── harness-check
├── tests/
└── .github/
    └── workflows/
```

说明：

- `AGENTS.md` 是唯一 Agent 入口文件。
- `HARNESS_QUICKSTART.md` 是初始化决策树。
- `templates/<project-type>/` 是可复制的最小目标项目 harness。
- `.harness/*.yaml` 是 durable policy source of truth。
- `docs/` 是解释层，不应成为 Agent 启动时唯一依赖。
- 根 `README.md` 负责目标项目文档落地路由；目录本地 `README.md` 负责具体写法。
- `docs/exec-plans/active/` 存放复杂任务的当前计划。
- `docs/exec-plans/completed/` 存放完成后的计划和决策日志。
- `docs/references/` 存放外部规范的本地摘要或链接。
- `scripts/` 中的命令应可被 Agent 直接调用。

## 4. 最低可用 Harness

一个项目至少应具备以下能力，才适合让 Agent 持续参与开发：

### 4.1 文档入口

- `README.md` 说明项目目标、安装、运行、测试。
- `AGENTS.md` 说明 Agent 入口地图和强制工作流。
- `ARCHITECTURE.md` 说明模块边界和依赖方向。
- `.harness/*.yaml` 说明机器可读策略。
- `docs/GIT_POLICY.md` 说明版本控制规则。

### 4.2 环境入口

- 独立依赖环境。
- 固定依赖版本。
- 本地安装命令。
- 本地启动命令。
- 本地测试命令。

示例命令命名：

```text
make setup
make lint
make test
make dev
```

### 4.3 验证入口

- 单元测试。
- 格式化或 lint。
- CI 中运行关键检查。
- 失败时输出足够日志。

### 4.4 任务入口

复杂任务应写执行计划。执行计划至少包含：

- 背景。
- 目标。
- 非目标。
- 验收标准。
- 风险。
- 实施步骤。
- 验证方式。
- 决策日志。

### 4.5 Git 入口

- `.gitignore` 覆盖当前 project type 的禁止内容。
- `docs/GIT_POLICY.md` 说明仓库边界和提交规则。
- `.harness/workspace-policy.yaml` 明确 `single-repo`、`mono-repo` 或 `mono-workspace`。
- `.harness/git-policy.yaml` 明确大文件、secret、artifact 和提交规则。

## 5. Agent 工作流

推荐标准流程：

1. 读取 `AGENTS.md` 和 `.harness/*.yaml`。
2. 判断 project type、workspace type 和 repository 边界。
3. 运行 `git status --short`。
4. 定位相关代码、测试和规格。
5. 明确任务目标和验收标准。
6. 做最小可行变更。
7. 运行本地验证。
8. 若失败，读取日志并修复。
9. 更新相关 policy、文档或测试。
10. 汇报变更、验证结果、git status 和剩余风险。

Bug 修复流程：

1. 复现问题。
2. 记录复现步骤。
3. 增加失败测试或回归用例。
4. 实施修复。
5. 证明测试从失败变为通过。
6. 更新相关文档。

新功能流程：

1. 写产品规格或执行计划。
2. 明确非目标。
3. 建立最小纵向切片。
4. 补充测试和观测。
5. 验证关键用户路径。
6. 更新 README、规格或架构文档。

## 6. 架构约束

Harness 应帮助 Agent 避免架构漂移。建议为项目定义明确的层次和依赖方向。

通用分层示例：

```text
types -> config -> repository -> service -> runtime -> ui
```

常见规则：

- 底层不得依赖高层。
- 跨领域访问必须走显式接口。
- 外部输入必须在边界处校验。
- 日志必须结构化。
- 共享逻辑优先进入公共模块。
- 禁止复制已有工具函数。
- 文件过大时必须拆分。

这些规则不应长期停留在文档里，应逐步进入 lint、结构测试或 CI。

## 7. 可观测性

Agent 需要直接读取运行状态，才能自己判断修复是否有效。

最低要求：

- 应用日志输出到稳定位置。
- 错误日志包含上下文和 trace id 或 request id。
- 测试失败信息可读。
- 本地服务启动失败时有明确错误。

进阶要求：

- 结构化日志，例如 JSON。
- 指标查询入口。
- tracing 查询入口。
- 本地临时观测栈。
- UI 截图或录屏验证。
- 性能预算和基准脚本。

## 8. 文档治理

文档需要像代码一样治理。

规则：

- 每个长期文档必须有明确用途。
- 每个目录应有索引或 README。
- 决策文档应记录日期和状态。
- 过期文档应删除、归档或标记。
- 文档和代码冲突时，应修正文档或代码，不保留模糊状态。

建议增加检查：

- Markdown 链接检查。
- 文档目录结构检查。
- 必需文档存在性检查。
- 过期 TODO 检查。
- 执行计划状态检查。

## 9. 质量与安全

质量标准：

- 所有关键路径必须有测试。
- 所有外部输入必须校验。
- 所有失败应可诊断。
- 所有公共接口应有清晰类型或 schema。
- 所有新增依赖应说明必要性。

安全标准：

- 不提交密钥。
- 不在日志中输出敏感信息。
- 不绕过认证和授权边界。
- 不引入无法审计的远程执行路径。
- 对文件、网络、命令执行能力设置边界。

可靠性标准：

- 关键流程有超时和重试策略。
- 后台任务有幂等性设计。
- 数据迁移可回滚或可重复执行。
- 错误处理保留足够上下文。

## 10. 合并与审查

Agent 高吞吐开发下，合并策略应关注恢复能力和反馈速度。

建议：

- PR 保持小而短。
- 关键检查必须自动化。
- 偶发失败应能重跑并保留日志。
- 审查意见中反复出现的问题应升级为规则。
- 合并后发现的问题应快速补丁，而不是长期阻塞无关变更。

不建议：

- 用人工记忆维持架构。
- 让大型 PR 长期悬挂。
- 把主观偏好作为唯一审查依据。
- 对所有问题使用同等严格的合并门。

## 10.1 Git Governance

Git governance 的核心问题是：什么属于仓库历史，什么属于外部 artifact。

MUST:

- 开工前运行 `git status --short`。
- 先确认 repository root，再执行 git mutation。
- 小步提交，提交信息使用 `feat`、`fix`、`refactor`、`docs`、`test`、`chore` 等类型。
- 将 `.gitignore` 作为 project type 的一部分维护。
- 把大文件、数据、模型、结果、secret 放到 artifact store 或本地非版本化位置。

NEVER:

- 默认在 workspace 根目录 `git init .`。
- 提交 `.venv/`、`node_modules/`、`dist/`、`build/`、`coverage/`。
- 提交 `data/`、`datasets/`、`models/`、`checkpoints/`、`outputs/`、`results/`。
- 提交 `.env`、token、private key、credential 或 `.DS_Store`。
- 用 destructive git 命令清理用户已有修改。

## 11. 熵管理

Agent 会复用仓库中已有模式，包括坏模式。必须定期清理漂移。

常见漂移：

- 重复工具函数。
- 目录层次失控。
- 文档过期。
- 测试只覆盖快乐路径。
- 日志格式不一致。
- 临时脚本变成事实依赖。
- 依赖方向被绕开。

治理方式：

- 维护技术债清单。
- 定期运行质量扫描。
- 把清理任务拆成小 PR。
- 把清理经验转化为 lint、测试或模板。
- 删除没人使用的文档、脚本和抽象。

## 12. 成熟度模型

### M0：空仓库

特征：

- 没有文档入口。
- 没有统一命令。
- 没有测试和 CI。

下一步：

- 建立 README、AGENTS 规则、policy layer、基础目录和验证命令。

### M1：可运行

特征：

- Agent 能安装依赖、启动项目、运行测试。
- 失败时能看到基础日志。

下一步：

- 增加架构文档、测试规范和 CI。

### M2：可约束

特征：

- 架构边界、命名、格式、测试要求由工具检查。
- 复杂任务有执行计划。

下一步：

- 增加结构测试、文档治理和质量评分。

### M3：可自我修复

特征：

- Agent 能复现问题、实施修复、验证用户路径、更新文档。
- 常见 review 意见已编码成规则。

下一步：

- 建立定期质量清理、观测查询和自动化回归流程。

### M4：可持续自治

特征：

- Agent 能端到端完成常规功能和修复。
- 人类主要负责优先级、判断和边界。
- 技术债被持续小步回收。

下一步：

- 优化控制系统、风险分级和人工介入点。

## 13. 项目启动清单

新项目应先完成：

- 创建 `README.md`。
- 创建 `AGENTS.md`。
- 如果是从 seed 初始化，先按 `HARNESS_QUICKSTART.md` 选择 `templates/<project-type>/`。
- 创建 `.harness/project-policy.yaml`。
- 创建 `.harness/workspace-policy.yaml`。
- 创建 `.harness/git-policy.yaml`。
- 创建 `.harness/quality-policy.yaml`。
- 创建 `.harness/architecture-policy.yaml`。
- 创建 `ARCHITECTURE.md`。
- 创建 `docs/GIT_POLICY.md`。
- 创建 `docs/product-specs/README.md`。
- 创建 `docs/product-specs/index.md`。
- 创建 `docs/exec-plans/README.md`。
- 创建 `docs/exec-plans/active/`。
- 创建 `docs/exec-plans/completed/`。
- 创建 `docs/exec-plans/tech-debt-tracker.md`。
- 创建 `docs/decisions/README.md`。
- 创建 `docs/references/README.md`。
- 创建 `docs/QUALITY.md`。
- 创建 `docs/RELIABILITY.md`。
- 创建 `docs/SECURITY.md`。
- 创建统一安装、测试、lint、启动、harness-check 命令。
- 配置最小 CI。
- 写第一组 smoke tests。

## 14. 任务模板

```markdown
# 任务标题

## 背景

说明为什么要做。

## 目标

列出必须完成的结果。

## 非目标

列出本次明确不处理的内容。

## 验收标准

- 可以被命令、测试、截图、日志或文档验证。

## 实施计划

1. ...
2. ...
3. ...

## 验证方式

- `make test`
- `make lint`

## 风险

列出风险和缓解方式。

## 决策日志

- YYYY-MM-DD：记录关键决定。
```

## 15. Review 清单

提交前检查：

- 是否满足任务验收标准。
- 是否运行了相关测试。
- 是否更新了相关文档。
- 是否引入了新的隐性约定。
- 是否复制了已有逻辑。
- 是否破坏了依赖方向。
- 是否泄露敏感信息。
- 是否留下无法解释的 TODO。

## 16. Harness 反模式

避免以下做法：

- 只写长 prompt，不改仓库结构。
- 把所有规则塞进一个巨大 `AGENTS.md` 文件。
- 只有软提示，没有 `.harness` policy layer。
- 让 Agent 依赖外部聊天记录。
- 没有测试就让 Agent 大规模改代码。
- 文档过期但没有检查机制。
- 只靠人工 review 维护架构。
- 没有日志和复现脚本就处理线上问题。
- 对反复问题只提醒，不编码成规则。

## 17. 落地顺序

推荐按以下顺序建设：

1. 判断 project type 和 workspace type。
2. 建立 `AGENTS.md`、`.harness/*.yaml` 和目录地图。
3. 建立安装、运行、测试、lint、harness-check 命令。
4. 建立 `.gitignore`、Git policy 和 artifact 边界。
5. 建立最小测试和 CI。
6. 写架构边界和依赖方向。
7. 把边界变成结构测试或 lint。
8. 建立执行计划和技术债目录。
9. 增加日志、指标和复现能力。
10. 定期把 review 经验转成工具规则。
11. 定期清理漂移和过期文档。

## 18. 判断标准

一个 harness 是否合格，可以用以下问题判断：

- 新 Agent 是否能在 5 分钟内知道项目如何运行和验证。
- Agent 是否能独立找到相关规格、架构和测试。
- 失败时是否有足够日志供 Agent 继续推进。
- 关键架构规则是否被工具强制执行。
- 反复出现的问题是否被编码成规则。
- 文档是否随着代码变化同步更新。
- 人类是否主要在判断目标和边界，而不是反复解释环境。

如果答案多数为否，应优先投资 harness，而不是继续增加一次性提示。
