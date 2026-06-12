# Reliability

The harness must make Agent work repeatable.

## 使用方式

什么时候读：

- 变更会影响启动、重试、超时、回滚、日志或观测能力时。
- 需要说明失败后如何诊断或恢复时。
- 本地验证无法完全覆盖运行风险时。

什么时候更新：

- 新增服务、后台任务、部署流程或外部依赖。
- 新增或改变超时、重试、幂等、回滚、日志、指标要求。
- 发现某类失败无法被 Agent 复现或诊断时。

MUST:

- Provide stable command entry points.
- Fail checks with clear messages and non-zero exit codes.
- Keep generated artifacts out of git.
- Document any manual verification that cannot be automated yet.

SHOULD:

- Keep policy schemas simple enough for Agents and scripts to inspect.
- Add CI once the guide is used by multiple active projects.
