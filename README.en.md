# BUPTapi-dsh — One-Click BUPT DSH Setup

[简体中文](README.md) · [Security policy](SECURITY.md)

A one-click DSH configuration and launcher tool for the Beijing University of Posts and Telecommunications (BUPT) campus-network environment, distributed as a single ZIP archive. Unzip the archive and double-click `bupt-dsh-setup.exe`; enter the BUPT API Key issued by the university, and the tool writes the DSH configuration and local credential, starts DSH Web, and opens the page in your browser. Current version: **1.0.0**.

## Features and boundaries

- One-click configuration: writes `settings.yaml` and `.credentials.yaml` automatically; no manual config editing.
- Private input: the BUPT API Key is not echoed while typing and is never written into the archive, source code, or normal logs.
- Automatic backup: existing configuration files are backed up with a timestamp (`.bupt-backup-<timestamp>`) before writing.
- Overwrite protection: writing is aborted when unmanaged top-level `llm-pi-ai:` or `agent-default-model:` sections are detected, so existing model configuration is never silently replaced.
- Endpoint probe: the BUPT endpoint is probed before launch (12 s timeout); if unreachable, a hint is printed and DSH still starts.
- Automatic launch: finds `dsh.cmd`, runs `dsh web --port 3080`, waits for the port, then opens `http://127.0.0.1:3080`.
- Updating the key: run the EXE again to update the Key and restart DSH; pass `--config-only` to only write the configuration.
- Boundaries: DSH / Node.js itself is not bundled; API Key issuance, account registration, and billing are out of scope.

## What is in the repository

| File | Description |
| --- | --- |
| `bupt-dsh-setup.zip` | Release archive (v1.0.0), SHA-256: `a78db726ac23115440f3201d29a73ed84ddacd2a029494b9c6d5f014c6dbfad5` |
| `README.md` | Chinese documentation |
| `README.en.md` | English documentation |
| `SECURITY.md` | Security policy |

Contents of the archive:

```text
bupt-dsh-setup.zip
├── bupt-dsh-setup.exe      # one-click setup and launcher (v1.0.0)
└── README.txt              # usage notes
```

## Requirements

- Windows 10 / 11 (verified on Windows 11).
- DSH / Node.js installed, with `dsh` available from the command line (i.e. `dsh.cmd` resolvable).
- BUPT campus network; connect to the campus VPN before use outside campus.

## Install and use

1. Download `bupt-dsh-setup.zip` and unzip it into any folder.
2. Double-click `bupt-dsh-setup.exe`.
3. Enter the BUPT API Key issued by the university (characters are not displayed while typing).
4. Wait for the browser to open the DSH page (`http://127.0.0.1:3080`).

Write the configuration only, without launching:

```text
bupt-dsh-setup.exe --config-only
```

- “北邮接口暂不可达” (BUPT endpoint unreachable): confirm the campus VPN is connected; DSH will still start.
- “未找到 dsh.cmd” (dsh.cmd not found): install DSH / Node.js first and run the EXE again.
- “DSH 未在预期时间内打开（端口 3080）” (DSH did not open in time): check the errors shown in the DSH window.

## Configuration and data

| Item | Default or location |
| --- | --- |
| Configuration directory | `%DSH_HOME%` (falls back to `%USERPROFILE%\.dsh`) |
| Model configuration | `settings.yaml` (managed block for provider `bupt`) |
| Local credential | `.credentials.yaml` (stores `BUPT_API_KEY`) |
| Backup of existing config | `settings.yaml.bupt-backup-<timestamp>`, `.credentials.yaml.bupt-backup-<timestamp>` |
| Model endpoint | `https://myai.bupt.edu.cn/llm-gw/v1`, model `deepseek-v4-flash` |
| Local page | `http://127.0.0.1:3080` |
| DSH runtime | the locally installed DSH / Node.js; not distributed with the tool |

## Security

- A BUPT API Key is an access credential issued by the university, equivalent to an account password. `.credentials.yaml` and its timestamped backups are sensitive files; never upload them, paste them into issues, or share them in debug packages.
- DSH / Node.js are not bundled. Install them from official sources and verify the checksum of downloaded files.
- Off-campus use requires the campus VPN. Do not connect to campus services from untrusted networks such as public Wi-Fi.
- Verify the SHA-256 of release archives (see the table above).
- Report security issues through GitHub private vulnerability reporting. See the [security policy](SECURITY.md).

## Boundaries

- The tool depends on the DSH command-line environment, campus-network reachability, and campus-side endpoint status. If the endpoint or configuration layout changes, startup or model availability may fail and needs manual review.
- The tool only configures and launches; it does not issue API Keys, manage accounts, or recover deleted configurations.
- Previous configurations are kept as timestamped backups; `.bupt-backup-*` files can be removed manually when rollback is no longer needed.

## License

The repository currently has no open-source license. Public visibility is not an open-source license grant; confirm permitted use and redistribution with the maintainer.
