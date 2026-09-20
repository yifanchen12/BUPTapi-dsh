# BUPTapi-dsh 北邮 DSH 一键接入

[English](README.en.md) · [安全策略](SECURITY.md)

面向北京邮电大学校园网环境的 DSH 一键配置与启动工具，以单个 ZIP 压缩包发布。解压后双击 `bupt-dsh-setup.exe`，输入学校发放的 BUPT API Key，程序即写入 DSH 配置与本地凭据、启动 DSH Web，并自动打开浏览器页面。当前版本：**1.0.0**。

## 功能与边界

- 一键配置：自动写入 `settings.yaml` 和 `.credentials.yaml`，无需手动编辑配置文件。
- 隐私输入：BUPT API Key 输入时不显示字符，也不会写入压缩包、源码或普通日志。
- 自动备份：写入前对已有配置生成带时间戳的备份（`.bupt-backup-<时间戳>`），便于回退。
- 覆盖保护：检测到已有未由本工具管理的 `llm-pi-ai:` 或 `agent-default-model:` 顶层配置时中止写入，避免覆盖原模型配置。
- 接口探活：启动前探测北邮接口（12 秒超时）；不可达时给出提示，仍会继续启动 DSH。
- 自动启动：查找 `dsh.cmd` 并执行 `dsh web --port 3080`，等待端口就绪后打开 `http://127.0.0.1:3080`。
- 更新密钥：再次运行本 EXE 可更新 Key 并重新启动 DSH；使用 `--config-only` 参数只写入配置、不启动。
- 边界：不包含 DSH / Node.js 本体，不负责 API Key 的申请与发放，不支持账号注册或缴费业务。

## 发布内容

| 文件 | 说明 |
| --- | --- |
| `bupt-dsh-setup.zip` | 发布压缩包（v1.0.0），SHA-256：`a78db726ac23115440f3201d29a73ed84ddacd2a029494b9c6d5f014c6dbfad5` |
| `README.md` | 中文文档 |
| `README.en.md` | 英文文档 |
| `SECURITY.md` | 安全策略 |

压缩包内部结构：

```text
bupt-dsh-setup.zip
├── bupt-dsh-setup.exe      # 一键配置与启动程序（v1.0.0）
└── README.txt              # 使用说明
```

## 环境要求

- Windows 10 / 11（已验证：Windows 11）。
- 已安装 DSH / Node.js，且命令行中可以运行 `dsh`（即能找到 `dsh.cmd`）。
- 北京邮电大学校园网络；校外使用前请先连接校园 VPN。

## 安装与使用

1. 下载 `bupt-dsh-setup.zip` 并解压到任意目录。
2. 双击 `bupt-dsh-setup.exe`。
3. 输入学校发放的 BUPT API Key（输入时不会显示字符）。
4. 等待浏览器自动打开 DSH 页面（`http://127.0.0.1:3080`）。

仅写入配置、不启动：

```text
bupt-dsh-setup.exe --config-only
```

- 提示“北邮接口暂不可达”：请确认校园 VPN 已连接；程序仍会继续启动 DSH。
- 提示“未找到 dsh.cmd”：请先安装 DSH / Node.js，再重新运行此 EXE。
- 提示“DSH 未在预期时间内打开（端口 3080）”：请查看 DSH 窗口中的错误信息。

## 配置与数据

| 项目 | 默认值或位置 |
| --- | --- |
| 配置目录 | `%DSH_HOME%`（未设置时为 `%USERPROFILE%\.dsh`） |
| 模型配置 | `settings.yaml`（以受管标记段落写入 provider `bupt`） |
| 本地凭据 | `.credentials.yaml`（保存 `BUPT_API_KEY`） |
| 原配置备份 | `settings.yaml.bupt-backup-<时间戳>`、`.credentials.yaml.bupt-backup-<时间戳>` |
| 模型接口 | `https://myai.bupt.edu.cn/llm-gw/v1`，模型 `deepseek-v4-flash` |
| 本地页面 | `http://127.0.0.1:3080` |
| DSH 本体 | 使用本机已安装的 DSH / Node.js，不随程序分发 |

## 安全

- BUPT API Key 属于学校颁发的访问凭据，等同账号口令；`.credentials.yaml` 及其时间戳备份均为敏感文件，不要上传、粘贴到 Issue 或随调试包公开分享。
- 本程序不包含 DSH 本体，请通过官方渠道安装 DSH / Node.js，并核对下载来源与 SHA-256 校验值。
- 校外使用必须连接校园 VPN；不要在公共 Wi-Fi、访客网络等不可信环境中直连校园接口。
- 下载发布压缩包后建议核对上文 SHA-256 摘要。
- 发现安全问题请使用 GitHub 私密漏洞报告，详见 [安全策略](SECURITY.md)。

## 适用边界

- 工具依赖 DSH 命令行环境、校园网络可达性与校园侧接口状态；接口地址或配置结构变化可能导致启动失败或模型不可用，需要人工复核。
- 工具只负责配置与启动，不提供 API Key 申请、账号管理或已删除配置的恢复能力。
- 原配置以带时间戳的备份文件保留；确认无需回退后可手动删除 `.bupt-backup-*` 文件。

## 授权

仓库目前未附带开源许可证；公开可见不表示授予开源许可，使用或分发前请向维护者确认授权范围。
