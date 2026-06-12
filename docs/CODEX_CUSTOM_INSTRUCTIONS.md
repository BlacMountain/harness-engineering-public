# Codex 自定义指令模板

以下内容可直接放入 Codex 自定义指令中。目标是让 Codex 在创建、初始化或维护工程时，先回到本地 harness 指导工程读取规则，并优先使用机器可读 policy，而不是只依赖聊天提示。

## 完整版

```text
创建、初始化或重构任何工程时，先参考本地 harness 指导工程：

/Users/wuyou/WorkingZone/harness-engineering

优先阅读：

1. /Users/wuyou/WorkingZone/harness-engineering/AGENTS.md
2. /Users/wuyou/WorkingZone/harness-engineering/HARNESS_QUICKSTART.md
3. /Users/wuyou/WorkingZone/harness-engineering/templates/

目标是从 Harness Seed Repository 初始化目标项目自己的本地 harness，而不是让目标项目长期依赖参考工程。初始化后，目标项目自己的 AGENTS.md、.harness/*.yaml 和 scripts/harness-check 才是长期约束源。

硬性规则：

1. AGENTS.md 是唯一 Agent 入口文件。
   - 默认创建 AGENTS.md，不创建 Agent.md。
   - AGENTS.md 应保持简短，只做入口地图、强制流程和关键禁令。
   - 详细规则放入 .harness/*.yaml 和 docs/*.md。

2. 每个长期工程应有目标本地 .harness policy layer。
   - .harness/project-policy.yaml：project type、必需文件、必需命令、环境隔离、文档结构。
   - .harness/workspace-policy.yaml：single-repo、mono-repo 或 mono-workspace，以及真实 repository 边界。
   - .harness/git-policy.yaml：什么进 git、什么不进 git、大文件规则、提交规则。
   - .harness/quality-policy.yaml：setup、lint、test、dev、验证汇报要求。
   - .harness/architecture-policy.yaml：目录边界、依赖方向和架构规则。

3. 新项目初始化前必须判断 project type。
   - source-repo：产品、服务、库代码仓库。
   - infra-repo：Terraform、Ansible、Helm、Kubernetes、Docker Compose 等基础设施仓库。
   - research-repo：实验代码、配置、论文复现、模型训练工程。
   - deployment-repo：部署编排、环境配置模板、发布脚本。
   - knowledge-repo：文档、规范、知识库，不强制虚拟环境。

4. 新项目初始化前必须判断 workspace type。
   - single-repo：当前目录就是一个 git repository。
   - mono-repo：一个 git repository 内含多个 package/service。
   - mono-workspace：一个 workspace 下有多个独立 git repository。
   - 如果 workspace/repository 边界不清晰，必须先向用户反馈，不允许直接 git init .。

5. 当用户提出任何以代码为目标的请求时，默认在当前目标工程目录执行初始化检查。
   - 先读取 .harness/*.yaml。
   - 如果 policy 缺失，先根据 HARNESS_QUICKSTART.md 判断 project type，并从 templates/<project-type>/ 初始化最低 policy layer。
   - Python 项目优先使用 .venv；其他语言按生态选择隔离方式。
   - knowledge-repo 或纯文档任务不强制创建虚拟环境。
   - 不要安装到全局环境，除非用户明确要求。

6. Git 和版本控制必须遵守 docs/GIT_POLICY.md。
   - 开工前运行 git status --short。
   - 不覆盖、不回滚用户已有修改。
   - 不提交 .venv、node_modules、dist、build、coverage、data、dataset、datasets、models、checkpoints、outputs、results、artifacts、.env、secret、.DS_Store。
   - 默认不提交超过 100MB 的文件。
   - 研究项目默认只把 code、configs、docs 和轻量 fixtures 放入 git。

7. 初始化工程时应提供统一命令入口。
   - setup
   - lint
   - test
   - dev 或 run
   - harness-check 或等价 policy 检查
   - 如果某个命令不适用，应输出清晰说明，而不是静默成功。

8. 长期规则不能只写在回复中。
   - 先沉淀到 .harness/*.yaml。
   - 能机械化的规则进入脚本、测试、lint 或 CI。
   - docs/*.md 负责解释规则和决策依据。
   - AGENTS.md 只保留任务启动时必须看到的短规则。

9. 完成后必须汇报：
   - 创建或修改了哪些文件。
   - 使用了哪些 harness policy。
   - 运行了哪些验证命令。
   - 当前 git status。
   - 哪些地方仍需用户确认或后续补齐。

如果无法访问 /Users/wuyou/WorkingZone/harness-engineering，应明确说明，然后使用最低模板继续：

- AGENTS.md
- .harness/project-policy.yaml
- .harness/workspace-policy.yaml
- .harness/git-policy.yaml
- .harness/quality-policy.yaml
- .harness/architecture-policy.yaml
- README.md
- ARCHITECTURE.md
- docs/GIT_POLICY.md
- docs/product-specs/index.md
- docs/exec-plans/active/
- docs/exec-plans/completed/
- docs/exec-plans/tech-debt-tracker.md
- docs/QUALITY.md
- docs/RELIABILITY.md
- docs/SECURITY.md
- 统一 setup、lint、test、dev、harness-check 命令入口
```

## 精简版

```text
创建或初始化任何工程时，先参考本地 harness 指导工程：/Users/wuyou/WorkingZone/harness-engineering。优先阅读 AGENTS.md、HARNESS_QUICKSTART.md 和 templates/。

AGENTS.md 是唯一 Agent 入口；不创建 Agent.md。该参考工程是 Harness Seed Repository：先读 AGENTS.md 和 HARNESS_QUICKSTART.md，判断目标项目类型，然后从 templates/<project-type>/ 初始化目标项目本地 harness。

新项目必须先判断 project type 和 workspace type。project type 包括 source-repo、infra-repo、research-repo、deployment-repo、knowledge-repo。workspace type 包括 single-repo、mono-repo、mono-workspace。边界不清晰时先反馈，不允许直接 git init .。

代码任务先执行初始化检查并读取目标项目本地 .harness policy。需要代码依赖时使用独立环境；Python 优先 .venv，其他语言按生态隔离。knowledge-repo 或纯文档任务不强制虚拟环境。

Git 必须遵守 docs/GIT_POLICY.md：开工前 git status --short；不覆盖用户修改；不提交 .venv、node_modules、dist、build、coverage、data、datasets、models、checkpoints、outputs、results、artifacts、.env、secret、.DS_Store；默认不提交超过 100MB 的文件。

完成后汇报文件变更、使用的 policy、验证命令、git status 和待确认事项。
```
