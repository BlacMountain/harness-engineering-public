# Codex 自定义指令模板

以下内容可直接放入 Codex 自定义指令中。目标是让 Codex 在创建、初始化或维护工程时，先回到本地 harness 指导工程读取规则，并优先使用目标项目本地 policy、docs 和验证命令。

## 完整版

```text
创建或初始化任何工程时，先参考本地 harness 指导工程：

/Users/wuyou/WorkingZone/harness-engineering

优先阅读：

1. /Users/wuyou/WorkingZone/harness-engineering/README.md
2. /Users/wuyou/WorkingZone/harness-engineering/AGENTS.md
3. /Users/wuyou/WorkingZone/harness-engineering/HARNESS_QUICKSTART.md
4. /Users/wuyou/WorkingZone/harness-engineering/docs/HARNESS_GUIDE.md

目标是从 Harness Seed Repository 学习一套统一 harness operating model，并在目标项目中创建目标本地 README.md、AGENTS.md、ARCHITECTURE.md、.harness/policy.yaml、docs 和验证命令。初始化后，目标项目自己的 AGENTS.md、.harness/policy.yaml、docs 和 scripts/harness-check 才是长期约束源。

硬性规则：

1. AGENTS.md 是 Agent 入口文件。
   - AGENTS.md 应保持简短，只做入口地图、强制流程和关键禁令。
   - 详细规则放入 .harness/policy.yaml、docs/*.md、脚本、测试、lint 或 CI。

2. 每个长期工程应有目标本地 .harness policy layer。
   - .harness/policy.yaml：统一保存结构、命令、workspace、Git、质量和架构机器规则。
   - policy.yaml 应保持为可被 harness-check 解析的受限 YAML 子集。

3. 新项目初始化前必须判断 workspace boundary 和 repository boundary。
   - single-repo：当前目录就是一个 git repository。
   - mono-repo：一个 git repository 内含多个 package/service。
   - mono-workspace：一个 workspace 下有多个独立 git repository。
   - 如果 workspace/repository 边界不清晰，必须先向用户反馈，不允许直接 git init .。

4. 当用户提出任何以代码为目标的请求时，默认在当前目标工程目录执行初始化检查。
   - 先读取目标项目本地 AGENTS.md 和 .harness/policy.yaml。
   - 如果 policy 缺失，先用 harness seed 的四层模型创建最低本地 harness。
   - Python 项目优先使用 .venv；其他语言按生态选择隔离方式。
   - knowledge-repo 或纯文档任务不强制创建虚拟环境。
   - 不要安装到全局环境，除非用户明确要求。

5. Git 和版本控制必须遵守 docs/GIT_POLICY.md。
   - 开工前运行 git status --short。
   - 不覆盖、不回滚用户已有修改。
   - 不提交 .venv、node_modules、dist、build、coverage、data、dataset、datasets、models、checkpoints、outputs、results、artifacts、.env、secret、.DS_Store。
   - 默认不提交超过 100MB 的文件。

6. 初始化工程时应提供统一命令入口。
   - setup
   - lint
   - test
   - dev 或 run
   - harness-check 或等价 policy 检查
   - 如果某个命令不适用，应输出清晰说明，而不是静默成功。

7. 长期规则不能只写在回复中。
   - 可机器解析的规则进入 .harness/policy.yaml。
   - 能机械化的规则进入脚本、测试、lint 或 CI。
   - docs/*.md 负责解释规则和决策依据。
   - AGENTS.md 只保留任务启动时必须看到的短规则。

8. 完成后必须汇报：
   - 创建或修改了哪些文件。
   - 使用了哪些 harness policy。
   - 运行了哪些验证命令。
   - 当前 git status。
   - 哪些地方仍需用户确认或后续补齐。

如果无法访问 /Users/wuyou/WorkingZone/harness-engineering，应明确说明，然后使用最低 harness 继续：

- README.md
- AGENTS.md
- ARCHITECTURE.md
- .harness/policy.yaml
- docs/product-specs/index.md
- docs/exec-plans/active/
- docs/exec-plans/completed/
- docs/exec-plans/tech-debt-tracker.md
- docs/decisions/index.md
- docs/references/index.md
- docs/GIT_POLICY.md
- docs/QUALITY.md
- docs/RELIABILITY.md
- docs/SECURITY.md
- 统一 setup、lint、test、dev、harness-check 命令入口
```

## 精简版

```text
创建或初始化任何工程时，先参考本地 harness 指导工程：/Users/wuyou/WorkingZone/harness-engineering。优先阅读 README.md、AGENTS.md、HARNESS_QUICKSTART.md 和 docs/HARNESS_GUIDE.md。

该参考工程是 Harness Seed Repository：先学习统一 harness operating model，再在目标项目创建目标本地 README.md、AGENTS.md、ARCHITECTURE.md、.harness/policy.yaml、docs 和验证命令。目标项目初始化完成后，以目标项目本地规则为长期真相源。

代码任务先执行初始化检查并读取目标项目本地 .harness policy。需要代码依赖时使用独立环境；Python 优先 .venv，其他语言按生态隔离。knowledge-repo 或纯文档任务不强制虚拟环境。

Git 必须遵守 docs/GIT_POLICY.md：开工前 git status --short；不覆盖用户修改；不提交 .venv、node_modules、dist、build、coverage、data、datasets、models、checkpoints、outputs、results、artifacts、.env、secret、.DS_Store；默认不提交超过 100MB 的文件。

完成后汇报文件变更、使用的 policy、验证命令、git status 和待确认事项。
```
