# Quality Policy

本文件定义 seed 仓库和目标项目 harness 的质量要求。机器可读规则优先写入 `.harness/quality-policy.yaml`，本文档负责说明规则的使用场景和人工判断标准。

## 使用方式

什么时候读：

- 新增或修改 `setup`、`lint`、`test`、`dev`、`harness-check`。
- 修改 `.harness/*.yaml`、目录结构、模板结构或目标项目文档落地路由。
- 完成任务后判断需要运行哪些验证命令。
- 将重复 review 意见升级为脚本、测试、lint 或 CI。

什么时候更新：

- 新增长期质量门槛。
- 改变验证命令、测试要求或完成报告要求。
- 发现 Agent 能绕过现有质量约束。

## 必守规则

- `AGENTS.md` 必须保持短入口，只保留启动阶段必须读取的规则。
- Durable constraints 必须先进入 `.harness/*.yaml`，再进入解释性文档。
- 结构规则应优先进入 `scripts/harness-check`。
- 修改 policy、文档结构、模板结构或命令入口后，必须运行 `scripts/harness-check`。
- 修改完成后必须汇报验证命令和当前 git 状态。

## 验证入口

```bash
scripts/harness-check
scripts/lint
scripts/test
```

## 禁止事项

- 不把长期规则只留在聊天回复中。
- 不新增无法从 `AGENTS.md`、根 `README.md` 或 `.harness/*.yaml` 找到的规则。
- 不创建没有验证路径的结构或模板。
