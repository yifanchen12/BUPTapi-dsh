# 安全声明 / Security Policy

## 支持范围 / Supported Versions

当前维护版本为 `1.0.0`。安全修复优先针对当前版本及 GitHub Release 中明确标注的版本。

The maintained version is `1.0.0`. Security fixes target the current version and any versions explicitly listed in GitHub Releases.

## 漏洞报告 / Reporting Vulnerabilities

请不要在公开 Issue 中发布可复现的漏洞细节、BUPT API Key、`.credentials.yaml` 及其备份内容、`%USERPROFILE%\.dsh` 下的配置或日志、包含个人路径的信息。

Do not post reproducible exploit details, BUPT API Keys, `.credentials.yaml` or its backups, configuration or logs from `%USERPROFILE%\.dsh`, or content containing personal paths in public issues.

优先使用 GitHub 仓库的私密漏洞报告功能：

https://github.com/yifanchen12/BUPTapi-dsh/security/advisories/new

报告时请提供 / Please provide:

- 受影响版本、操作系统和 DSH / Node.js 版本；
- 最小复现步骤以及预期、实际行为；
- 删除敏感内容后的必要日志或截图；
- 影响范围、利用条件和可能的修复方向。

提交前请再次确认材料中不含 API Key、Token、个人目录或完整配置内容。

## 运行安全要求 / Runtime Security Requirements

- BUPT API Key 是学校颁发的访问凭据，等同账号口令。不要截图、公开发布或提交到仓库。
- `.credentials.yaml` 及其时间戳备份（`.bupt-backup-*`）均包含密钥，属于敏感文件；不得上传、粘贴到 Issue 或随调试压缩包公开分享。
- 只建议在可信网络中运行（校园网或校园 VPN）；不要在公共 Wi-Fi、访客网络中直连校园接口。
- DSH / Node.js 本体请通过官方渠道安装；本工具不包含也不分发 DSH。
- 提示“北邮接口暂不可达”时，先确认校园 VPN；提示“未找到 dsh.cmd”时，检查 DSH / Node.js 安装与命令行 PATH。
- 下载发布压缩包后请核对 SHA-256 摘要，防止传输过程中被替换。

## 安全设计说明 / Security Design Notes

- API Key 输入时不显示字符；密钥不写入压缩包、源码或普通日志。
- 配置与凭据写入当前用户的配置目录（`%DSH_HOME%`，未设置时为 `%USERPROFILE%\.dsh`）：模型配置写入 `settings.yaml`，密钥写入 `.credentials.yaml`。
- 写入前对 `settings.yaml` 与 `.credentials.yaml` 自动生成带时间戳的备份（`.bupt-backup-<时间戳>`），便于回退。
- 若检测到已有未由本工具管理的 `llm-pi-ai:` 或 `agent-default-model:` 顶层配置，写入会中止，避免覆盖原有模型配置。
- 启动前使用 Bearer 认证探测 `https://myai.bupt.edu.cn/llm-gw/v1/models`（12 秒超时）；探活失败不阻止 DSH 启动。
- DSH Web 以 `dsh web --port 3080` 启动，本地页面为 `http://127.0.0.1:3080`，等待端口就绪最长 35 秒。
- 程序没有遥测、广告 SDK 或第三方统计服务；除校园侧接口探活与 DSH 自身连接外，不与其他外部服务通信。

## 已知边界 / Known Limitations

- BUPT API Key 以明文形式保存在本机 `.credentials.yaml`，时间戳备份同样包含明文密钥；本工具不提供加密存储。在共享计算机上，其他账户可能读取该用户目录下的配置，请使用私人账户并妥善管理本机文件权限。
- 工具不具备端到端加密或身份认证机制，不能防范已控制本机的攻击者。
- 校园侧接口地址、认证策略或 DSH 配置结构变化可能导致探活失败、模型不可用或启动失败，需要人工复核。
- 本策略仅覆盖本仓库分发的发布压缩包；DSH 本体及 Node.js 的安全由各自官方渠道负责。

---

The maintained version is `1.0.0`. Report issues privately and never include BUPT API Keys, credential backups, or personal paths. The tool writes the Key and model configuration into `%DSH_HOME%` (default `%USERPROFILE%\.dsh`) with timestamped backups, protects unmanaged model configuration from being overwritten, does not bundle or distribute DSH itself, has no telemetry, and is intended for use on the BUPT campus network or VPN only.
