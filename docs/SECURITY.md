# Security Policy

本文件定义 harness 中 secret、权限、输入边界、外部执行和仓库边界的安全要求。

## 使用方式

什么时候读：

- 处理 secret、token、credential、cookie、private key 或 `.env`。
- 修改认证、授权、权限、日志、外部输入或远程执行路径。
- 引入新依赖、脚本、网络访问、第三方服务或自动化工具。

什么时候更新：

- 新增敏感数据类型、安全边界或 secret 管理方式。
- 发现日志、测试、文档、脚本或示例可能泄露敏感信息。
- `.gitignore`、运行脚本或安全规则变化。

## 必守规则

- secret 和本地环境文件不得进入 git。
- 日志、测试输出和文档不得包含 token、cookie、private key 或凭据。
- 外部输入必须视为不可信。
- 执行远程脚本或下载外部二进制前，必须有明确来源和信任边界。
- 执行 git mutation 前必须确认 repository boundary。

## 禁止事项

- 不提交 `.env`、`.env.*`、token、private key、cookie 或凭据文件。
- 不把敏感信息作为示例值写入模板。
- 不绕过认证、授权或权限边界来完成任务。
- 不添加无法审计来源的远程执行路径。
