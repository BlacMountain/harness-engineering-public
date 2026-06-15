# 从多模板收敛到单一 Harness 工程化方案

## 结论

你现在要解决的核心问题，不是先把仓库做得“轻量”还是“最小”，而是先把 **harness 工程化方案本身定义清楚**：它的入口在哪里，知识在哪里沉淀，哪些规则是给 Agent 读的，哪些规则是机器要执行的，哪些东西进入目标项目后会成为本地长期真相源。就这个目标而言，把仓库从“按 project type 选择不同模板”收敛到“一套统一的 harness operating model”，方向是对的。模板工具的官方定位本来就是**启动新项目**或**快速脚手架**，而不是持续治理已有项目；同时，软件复用研究也长期表明，多变体的 clone-and-own 路线虽然前期便宜灵活，但会随着变体数量上升而带来更高维护成本，而共享平台或共享过程会把复杂度收敛到中心。citeturn23view0turn5search1turn5search2turn28view0turn15search2

真正重要的也不是“少写点字”本身，而是**分层和职责清晰**。OpenAI 关于 harness engineering 的总结、Anthropic 关于 `CLAUDE.md` 与 `.claude/rules/` 的文档、GitHub Copilot 对仓库级/路径级/Agent 级指令的设计，实际上都在收敛到同一种模式：**短入口负责路由，仓库内文档负责系统事实，局部规则负责局部约束，脚本与设置负责强制执行**。这说明你这个仓库最应该先定义的是一套单一、稳定、能迁移到目标项目的工程化结构，而不是继续扩展模板矩阵。citeturn25view0turn19view0turn10view0turn24view2turn19view2

## 多模板为什么不应成为中心

从官方工具定位看，模板机制天然偏向“新项目起步”。GitHub 模板仓库的文档明确把它定义为“从已有仓库生成一个新仓库”，而且模板生成出的分支历史与源模板**彼此无关**；Yeoman 把自己定义为“scaffolding tool”，Cookiecutter 也把核心价值表述为“快速开始新项目”。这类工具非常适合 greenfield bootstrap，但它们不是为“让 Agent 学一套方法，然后把方法应用到已有项目和不同阶段项目”而设计的。你的实际目标如果是治理 project lifecycle，而不是把目录复制过去，那么模板就不该成为仓库中心。citeturn23view0turn5search1turn5search2

从软件工程研究看，多套 harness 部署方案会落入和软件变体类似的治理问题。ESEC/FSE 2020 的实证研究把“clone & own”和“platform-oriented reuse”做了对比：前者初始便宜、灵活，但**不随复用频次扩展**，并带来高维护成本；后者虽然前期定义成本更高，但后续复用成本更低、代码质量更高。2024 年《Journal of Systems and Software》的研究还指出，企业在处理变体时经常缺少共同需求与应用需求文档，而**标准化过程**有助于复用组件。把这套发现放到你的场景里，就是：你不该继续维护多种 harness 变体，而应该先定义一个**共享的工程化平台/过程**，再把差异下沉到局部政策。citeturn28view0turn29view0turn29view1

这并不意味着“差异消失了”。Research 项目、软件项目、部署项目之间依然会有 Git 边界、质量门槛、可靠性要求、安全约束上的差异。但这些差异不必在源仓库里通过多套模板表达。GitHub Spec Kit 的设计就很有代表性：它提供的是一个**统一核心流程** `Spec → Plan → Tasks → Implement`，再通过 preset、extension 和 workflow 去扩展，而不是为每种项目类型维护一棵不同的主树。对你来说，这意味着应该把差异放到 **target-local policy delta**，而不是 `templates/<project-type>/`。citeturn27view0turn20view0turn28view0

## Agent 时代已经出现的共识模式

OpenAI 在 2026 年的 harness engineering 文章里，几乎完整给出了你这个仓库现在需要的认识模型：`AGENTS.md` 不应该是百科全书，而应该是**目录和地图**；真正的系统事实应该存在结构化的 `docs/` 中；`product-specs/`、`exec-plans/`、`references/`、架构和质量/安全/可靠性文档一起构成版本化、可检查、可被 Agent 直接读取的知识库。它们把 plans 当作一等工件，把 active/completed 的执行计划都放进仓库，并且用 lint/CI 检查知识库是否过时、是否互相链接、是否结构正确。这个思路和你现在想把仓库定义成“方法仓库”而不是“模板仓库”的方向高度一致。citeturn25view0turn26view1

同一篇文章还给出了一条对你特别重要的判断：**从 Agent 的视角，运行时看不到的东西就等于不存在**。Google Docs、聊天记录、脑内知识都不算；真正能被 Agent 使用的，是 repo-local、versioned 的代码、Markdown、schema 和 executable plans。也就是说，如果你要定义的是 harness 工程化方案，那么这个方案必须优先定义“哪些知识必须以仓库内工件形式存在”。因此，你现在对 `docs/product-specs`、`docs/exec-plans`、`docs/decisions`、`docs/references` 的重视是对的，因为这些目录不是普通文档分类，而是 Agent 可直接读取的项目运行面。citeturn25view0

与此同时，Agent 入口文件必须克制。Anthropic 文档明确建议 `CLAUDE.md` 里的指令要**具体、简洁、结构化**，目标是每个文件小于约 200 行；对于只在某些目录或文件类型下成立的规则，应转移到 path-scoped rules。GitHub Copilot 也把规则分成 repository-wide、path-specific、agent instructions 三层，并明确提醒尽量避免相互冲突的多套指令。更重要的是，2026 年 2 月的一项 AGENTS.md 评估研究发现，不必要的 repository context 往往会降低任务成功率并增加 20% 以上成本，因此作者建议人写的 context files 应只保留**最必要的要求**。所以，你最近“不要反复讲这是个什么仓库，而要直说让 Agent 怎么做、不要重复说同一句话”的修改方向，不只是风格偏好，而是和当前证据一致。citeturn19view1turn10view0turn24view2turn16view0

## 建议采用的 Harness 工程化方案

如果你现在要先把方案定义清楚，我建议把整个 harness 明确成一个**四层模型**，而不是再围绕模板和项目类型组织仓库。这四层分别是：**路由层、知识层、政策层、执行层**。这种分层同时吸收了 OpenAI 的“AGENTS 是地图、docs 是 system of record”、Anthropic 的“用 path-scoped rules 减少噪声”、GitHub Copilot 的“repo/path/agent instructions 分层”，以及 OPA 的“policy 与 enforcement 解耦”的思想。citeturn25view0turn10view0turn24view2turn24view3

第一层是**路由层**。这里包括根 `README.md`、根 `AGENTS.md`、`HARNESS_QUICKSTART.md`。它们的职责不是解释一切，而是告诉 Agent：这个仓库定义了一套什么方法、先看什么、再看什么、进入目标项目后要做哪些检查。OpenAI 明确主张把 `AGENTS.md` 当作 table of contents；Anthropic 和 GitHub 也都把这类文件放在仓库上下文入口。你的 README 第一屏应该因此从“自我介绍”转为“操作路由图”。citeturn25view0turn24view2turn19view0

第二层是**生命周期知识层**。这层是仓库真正的系统事实，应该包含 `product-specs/`、`ARCHITECTURE.md`、`exec-plans/`、`decisions/`、`references/`，以及横切治理文档 `GIT_POLICY.md`、`QUALITY.md`、`RELIABILITY.md`、`SECURITY.md`。这和 Spec Kit 的 `Spec → Plan → Tasks → Implement` 哲学完全一致，只是你用的是更贴近项目实践的命名：`product-specs` 对应 what，`exec-plans` 对应 how，`decisions` 负责 why，`references` 负责外部依据，而治理文档负责长期约束。OpenAI 的文章也明确把 plans、quality、reliability、security 这些都放进仓库知识库中。citeturn27view0turn20view0turn25view0

第三层是**政策层**。这一层如果存在，就放在 `.harness/`，但它的意义不能再模糊。它不是第二套 prose docs，不是 session diary，不是计划归档区，而是**机器可读、结构稳定、预期会被工具消费的政策表达层**。从 OPA 到 Claude 的 managed settings / hooks，都在强调同一个原则：把 policy 与 enforcement 解耦，让机器读取结构化规则，再由执行器去判断是否允许、是否通过。换句话说，`.harness/` 只有在你要表达结构化约束、并打算让脚本/CI/Agent 读取时才有价值。citeturn24view3turn19view2

第四层是**执行层**。这里是 `scripts/`、`harness-check`、lint、test、CI、hooks，以及所有会把“文档里的长期要求”升级成“程序会检查的规则”的东西。OpenAI 明确说过：当 documentation 不够时，就把规则**promote into code**；Anthropic 也把行为指导与技术强制执行分开：`CLAUDE.md` 负责 guide，settings 和 hooks 负责 enforce。因此，真正成熟的 harness 不是“把所有规则都写在文档里”，而是让文档、政策、脚本形成闭环。citeturn26view0turn19view2

可以把这套方案理解成下面这个拓扑：

```text
User / Task
  ↓
README.md / AGENTS.md / HARNESS_QUICKSTART.md
  ↓
docs/ as system of record
  ├── product-specs/
  ├── ARCHITECTURE.md
  ├── exec-plans/
  ├── decisions/
  ├── references/
  ├── GIT_POLICY.md
  ├── QUALITY.md
  ├── RELIABILITY.md
  └── SECURITY.md
  ↓
.harness/ as machine-readable policy
  ↓
scripts/ + hooks + CI as enforcement
```

这不是“最小版” harness，而是一个**定义完整、层次明确**的 harness engineering 方案。它的重点不是少文件，而是每一层都知道自己负责什么。

## `.harness` 的职责边界

你问“`.harness` 到底有什么用”，真正的答案是：**它只有在承担政策层时才有用**。OPA 把“policy as code”定义为用声明式规则描述系统约束，并把 policy decision-making 从 enforcement 中剥离出来；Anthropic 也区分了 `CLAUDE.md` 这样的行为指导，和 settings/hooks 这样的硬执行层。按照这个思路，`.harness` 最合理的定义，就是你这套 harness 里的**machine-readable policy plane**：它描述结构化约束，但不负责长篇解释，也不直接执行。citeturn24view3turn19view2

因此，适合放进 `.harness/` 的内容，是那些“可以被程序可靠读取”的东西。例如：目标项目的 repo 边界、必须运行的检查、允许生成的目录、禁止提交的路径、某些文档是否为必需、质量门槛是否需要某个命令输出、某些目录是否需要路径级规则。你完全可以把它设计成一个未来可扩展的政策层，而不是今天就全部实现完。关键是先把**语义**定义好：`.harness` 存的不是说明书，而是结构化规则。这个定位本身就会让后续仓库演进更清楚。citeturn24view3turn19view2

反过来说，不适合进 `.harness/` 的，是产品说明、计划正文、决策理由、外部参考摘要、团队文化解释这类 prose knowledge。它们应该留在 `docs/` 和目录内 README 中，因为 OpenAI 的 harness 总结已经非常清楚：Agent 可共享的长期知识应该进入仓库文档系统；Anthropic 也说明 auto memory 是 machine-local、不会跨机器共享。共享、版本化、可审查的 durable knowledge 应留在 repo docs，而不是混进一个看起来像配置目录的地方。citeturn25view0turn19view2

换句话说，`.harness/` 在你的方案里不是必须是“大而全”的，但**必须被准确命名**。它的准确职责可以概括成一句话：**把会被机器消费的长期项目政策放进这里**。如果你现在先定义好这个角色，即便第一版只放很少的文件，也不会再陷入“它是不是第二个 docs”这种模糊状态。citeturn24view3turn19view2

## 对这个仓库的定稿建议

如果按照上面的方案来定稿，这个仓库更适合被定义为 **Harness Method Repository** 或 **Harness Operating Model Repository**，而不是 Template Repository。它的工作不是按项目类型复制骨架，而是教 Agent 如何在目标项目里建立：入口、文档体系、政策层和验证层。GitHub Spec Kit 的一条重要启发就是：统一核心流程并不排斥扩展，但扩展应通过 preset/extension 或 target-local delta 进入，而不是先在源仓库做出五套平行主树。citeturn27view0turn20view0turn28view0

从仓库结构看，我会建议主干收敛成下面这种形式：

```text
harness-engineering/
├── README.md
├── AGENTS.md
├── HARNESS_QUICKSTART.md
├── docs/
│   ├── HARNESS_GUIDE.md
│   ├── GIT_POLICY.md
│   ├── QUALITY.md
│   ├── RELIABILITY.md
│   ├── SECURITY.md
│   ├── product-specs/
│   │   └── README.md
│   ├── exec-plans/
│   │   └── README.md
│   ├── decisions/
│   │   └── README.md
│   └── references/
│       └── README.md
├── .harness/          # 仅当它承载 machine-readable policy 时保留
└── scripts/
    └── harness-check
```

这里最关键的不是目录名，而是入口写法。根 `README.md` 第一屏应该回答三个动作问题：**这套 harness 如何工作、Agent 进入目标项目后按什么顺序做事、知识与政策分别落到哪里**。根 `AGENTS.md` 则只保留启动协议和强制动作，不再重复解释哲学。对 `product-specs/`、`exec-plans/`、`decisions/`、`references/`，则用目录 README 解释“什么时候写、写什么、不要写什么、怎么维护”。这和 OpenAI 的“短 AGENTS + docs system of record”、Anthropic 的“具体且简洁”、GitHub 的“repo/path/agent 指令分层”是一致的。citeturn25view0turn19view1turn24view2turn16view0

至于 `project type`，我建议把它**移出主控制面**。也就是说，不再让 Agent 先判断自己要套哪套 harness，而是统一学习一套方法，然后在目标项目本地用 `GIT_POLICY.md`、`QUALITY.md`、`SECURITY.md`、`RELIABILITY.md` 和必要的 `.harness` 政策去表达差异。以后如果你确实发现某些差异反复出现，可以再把它们抽象为 preset 或 policy profile，而不是回到多模板部署模型。这样做的重点不是“更轻”，而是**先把中心定义成一套清晰的工程化方案，再把差异放到边缘**。citeturn27view0turn28view0turn29view0

## 最终判断

如果把你的目标表述成一句话，我认为最合适的版本是：

**这个仓库不是为了按项目类型分发不同 harness 模板，而是为了定义一套统一的 harness 工程化 operating model，使 Agent 能在任意目标项目中建立可读、可维护、可验证的项目治理结构。**

在这个定义下，最需要先做清楚的四件事是：一是让 `README + AGENTS + QUICKSTART` 成为明确的入口路由；二是把 `docs/` 变成真正的 lifecycle system of record；三是把 `.harness/` 定义成 machine-readable policy plane，而不是第二个文档区；四是让 `scripts/` 与 CI 承担 enforcement，把成熟规则从文档提升为代码。只要这四件事确定，你的 harness 工程化方案就已经成型了；轻量还是完整、是否立刻铺满 `.harness/`、以后要不要加 preset，都是下一层实现问题，而不是方案定义问题。citeturn25view0turn24view3turn19view2turn27view0turn16view0