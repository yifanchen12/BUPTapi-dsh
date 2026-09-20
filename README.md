# BUPTapi-dsh 北邮 DSH 一键接入

[English](README.en.md) · [安全策略](SECURITY.md)

面向北京邮电大学校园网环境的 DSH 一键接入工具集，提供 Windows 与 macOS 两个平台的配置包和安装包。解压后双击对应入口，输入学校发放的 BUPT API Key，程序即写入 DSH 配置与本地凭据、启动 DSH Web，并自动打开浏览器页面。当前版本：**1.0.0**。

## 功能与边界

- 一键配置（`bupt-dsh-setup.exe`）：自动写入 `settings.yaml` 和 `.credentials.yaml`，无需手动编辑配置文件。
- 一键安装（`dsh-download-setup.exe`）：自动下载并安装 Node.js v24.19.0 与 DSH（`@deepseek-ai/dsh`），全程无需管理员权限；检测到已安装则自动复用。
- 官方来源：Node.js 取自 nodejs.org 官方分发并核对官方 `SHASUMS256.txt`，DSH 自 npm 官方仓库安装。
- 隐私输入：BUPT API Key 输入时不显示字符，也不会写入压缩包、源码或普通日志。
- 自动备份：写入前对已有配置生成带时间戳的备份（`.bupt-backup-<时间戳>`），便于回退。
- 配置合并：已有官方或其他 Provider 路由会保留，只追加北邮校园模型；默认模型切换前会自动备份原配置。
- macOS 支持：提供 Apple Silicon（arm64）与 Intel（x86_64）的 `.command` 一键入口。
- 接口探活：启动前探测北邮接口（12 秒超时）；不可达时给出提示，仍会继续启动 DSH。
- 自动启动：查找 `dsh.cmd` 并执行 `dsh web --port 3080`，等待端口就绪后打开 `http://127.0.0.1:3080`。
- 更新密钥：再次运行配置 EXE 可更新 Key 并重新启动 DSH；使用 `--config-only` 参数只写入配置、不启动。
- 边界：不负责 API Key 的申请与发放，不支持账号注册或缴费业务。

## 发布内容

| 文件 | 说明 |
| --- | --- |
| `dsh-install.zip` | 一键安装包（v1.0.0，61.23 MB）：自动安装 DSH 后进入配置流程，适合未安装 DSH 的新机。SHA-256：`1500f5b3393ecd918eb7d3e06374b8f43f5de7e62f570a867301d46bdd1552f0` |
| `bupt-dsh-setup.zip` | Windows 一键配置包（v1.0.0，30.61 MB）：面向已安装 DSH 的环境，直接配置并启动。SHA-256：`23e7dd63a53fc4033e428be229b4b07f601c25e01610be0356f44d3391f94cec` |
| `dsh-install-mac.zip` | macOS 一键安装包：自动下载 Node.js、安装 DSH，再完成配置。SHA-256：`c7ee78eff80066bb37a2d2f15b82e07dc216809e2108bd0b29fc8d07ca0dd0d1` |
| `bupt-dsh-setup-mac.zip` | macOS 一键配置包：面向已安装 Node.js/DSH 的环境。SHA-256：`99986686a5eee0ca499ad99958670cfaad3b2146f2a10856bdbb85be276f705d` |
| `README.md` | 中文文档 |
| `README.en.md` | 英文文档 |
| `SECURITY.md` | 安全策略 |

压缩包的内部结构：

```text
dsh-install.zip                    bupt-dsh-setup.zip
├── dsh-download-setup.exe        ├── bupt-dsh-setup.exe
├── bupt-dsh-setup.exe            └── README.txt
└── README.txt

dsh-install-mac.zip                bupt-dsh-setup-mac.zip
├── dsh-download-setup.command     ├── bupt-dsh-setup.command
├── configure.mjs                  ├── configure.mjs
└── README.txt                     └── README.txt
```

## 环境要求

- Windows 10 / 11（已验证：Windows 11），64 位。
- macOS 12 或更高版本；支持 Apple Silicon 与 Intel。
- `dsh-install.zip`：无需预装任何软件，仅需可访问外网（预计下载 50–80 MB），安装全程无需管理员权限。
- `bupt-dsh-setup.zip`：需要已安装 DSH / Node.js，且命令行中可以运行 `dsh`（即能找到 `dsh.cmd`）。
- 北京邮电大学校园网络；校外使用前请先连接校园 VPN。

## 安装与使用

新生快速开始（推荐使用 `dsh-install.zip`）：

1. 下载 `dsh-install.zip` 并解压到任意目录。
2. 双击 `dsh-download-setup.exe`，等待自动安装（约 3–10 分钟，视网络而定），全程无需管理员权限。
3. 安装完成后按提示输入 `Y` 进入配置流程；输入学校发放的 BUPT API Key（输入时不显示字符）。
4. 等待浏览器自动打开 DSH 页面（`http://127.0.0.1:3080`）。

macOS 用户：

1. 未安装 DSH / Node.js：下载 `dsh-install-mac.zip`，解压后双击 `dsh-download-setup.command`。
2. 已安装 DSH / Node.js：下载 `bupt-dsh-setup-mac.zip`，解压后双击 `bupt-dsh-setup.command`。
3. 如果 macOS 首次阻止运行，请右键 `.command` 文件选择“打开”；仍被阻止时在终端执行 `chmod +x *.command`。
4. 输入学校发放的 BUPT API Key，等待浏览器打开 DSH 页面。

已安装 DSH / Node.js 的用户：

1. 下载 `bupt-dsh-setup.zip` 并解压到任意目录。
2. 双击 `bupt-dsh-setup.exe`。
3. 输入学校发放的 BUPT API Key（输入时不显示字符）。
4. 等待浏览器自动打开 DSH 页面（`http://127.0.0.1:3080`）。

仅写入配置、不启动：

```text
bupt-dsh-setup.exe --config-only
```

- 提示“北邮接口暂不可达”：请确认校园 VPN 已连接；程序仍会继续启动 DSH。
- 提示“未找到 dsh.cmd”：请先运行 `dsh-download-setup.exe` 完成安装，或手动安装 DSH / Node.js 后重试。
- 提示“DSH 未在预期时间内打开（端口 3080）”：请查看 DSH 窗口中的错误信息。

## 配置与数据

| 项目 | 默认值或位置 |
| --- | --- |
| 配置目录 | `%DSH_HOME%`（未设置时为 `%USERPROFILE%\.dsh`） |
| 模型配置 | `settings.yaml`（以受管标记段落写入 provider `bupt-campus`） |
| 本地凭据 | `.credentials.yaml`（保存 `BUPT_API_KEY`） |
| 原配置备份 | `settings.yaml.bupt-backup-<时间戳>`、`.credentials.yaml.bupt-backup-<时间戳>` |
| 模型接口 | `https://myai.bupt.edu.cn/llm-gw/v1`，模型 `deepseek-v4-flash` |
| 本地页面 | `http://127.0.0.1:3080` |
| 安装位置 | 一键安装包将 Node.js / DSH 安装到用户目录 `%LOCALAPPDATA%\Programs\nodejs`（含 `node.exe` 与 `dsh.cmd`），并把该目录追加到当前用户 `PATH`（已存在则不重复添加） |

## 安全

- BUPT API Key 属于学校颁发的访问凭据，等同账号口令；`.credentials.yaml` 及其时间戳备份均为敏感文件，不要上传、粘贴到 Issue 或随调试包公开分享。
- `dsh-install.zip` 中的安装器只从 nodejs.org 官方地址下载 Node.js（核对官方 `SHASUMS256.txt`），再从 npm 官方仓库安装 DSH；不捆绑任何第三方软件，无遥测。
- 校外使用必须连接校园 VPN；不要在公共 Wi-Fi、访客网络等不可信环境中直连校园接口。
- 下载发布压缩包后建议核对上表 SHA-256 摘要。
- 发现安全问题请使用 GitHub 私密漏洞报告，详见 [安全策略](SECURITY.md)。

## 适用边界

- 工具依赖 DSH 命令行环境、校园网络可达性与校园侧接口状态；接口地址或配置结构变化可能导致启动失败或模型不可用，需要人工复核。
- `dsh-download-setup.exe` 需要访问 nodejs.org 与 npm registry；校内网络若屏蔽外网，安装环节会失败，请先连接外网（或可用代理）再运行。
- 工具只负责配置与启动，不提供 API Key 申请、账号管理或已删除配置的恢复能力。
- 原配置以带时间戳的备份文件保留；确认无需回退后可手动删除 `.bupt-backup-*` 文件。

## 授权

仓库目前未附带开源许可证；公开可见不表示授予开源许可，使用或分发前请向维护者确认授权范围。
