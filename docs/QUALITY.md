# Quality

Quality rules define the minimum verification loop for this harness guide.

## 使用方式

什么时候读：

- 新增或修改验证命令、测试要求、lint 或 CI 时。
- 完成任务后判断需要运行哪些验证时。
- 把 review 意见升级成可执行检查时。

什么时候更新：

- 新增长期质量门槛。
- 改变 `setup`、`lint`、`test`、`dev` 或 `harness-check` 行为。
- 新增必须在完成报告中说明的验证信息。

MUST:

- Keep `AGENTS.md` short and task-start readable.
- Keep durable rules in `.harness/*.yaml` before explaining them in docs.
- Run `scripts/harness-check` after changing policy, docs structure, or command
  entry points.
- Update docs when policy changes.

SHOULD:

- Convert repeated review comments into policy or executable checks.
- Prefer checks with actionable failure messages.

NEVER:

- Leave durable rules only in chat replies.
- Add a policy that cannot be located from `AGENTS.md`.
