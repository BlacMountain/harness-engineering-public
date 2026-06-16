# Quality Policy

本文件定义 seed 仓库和目标项目 harness 的质量要求。本文档负责说明质量规则、验证入口、完成标准和人工判断边界；可机械化的规则应进入 `scripts/lint`、`scripts/test`、hooks 或 CI。

## 使用方式

什么时候读：

- 新增或修改 `setup`、`lint`、`test`、`dev`。
- 修改目录结构、验证脚本或目标项目文档落地路由。
- 完成任务后判断需要运行哪些验证命令。
- 将重复 review 意见升级为脚本、测试、lint 或 CI。

什么时候更新：

- 新增长期质量门槛。
- 改变验证命令、测试要求或完成报告要求。
- 发现 Agent 能绕过现有质量约束。

## 必守规则

- `AGENTS.md` 必须保持短入口，只保留启动阶段必须读取的规则。
- Durable knowledge 和项目周期记录必须进入 `docs/`。
- 可机械化的结构规则应进入 `scripts/lint`、`scripts/test`、hooks 或 CI。
- 计划或文档之间存在冲突时，未决事项必须记录到执行计划、技术债或决策文档。
- 修改文档结构、模板结构或命令入口后，必须运行 `scripts/lint` 和 `scripts/test`。
- 修改完成后必须汇报验证命令和当前 git 状态。

## 验证入口

```bash
scripts/lint
scripts/test
```

## 禁止事项

- 不把长期规则只留在聊天回复中。
- 不新增无法从 `AGENTS.md`、根 `README.md`、`docs/` 或验证脚本找到的规则。
- 不创建没有验证路径的结构或模板。
- 不重新引入多套 seed 启动路径，除非先记录新的架构决策并更新检查脚本。
