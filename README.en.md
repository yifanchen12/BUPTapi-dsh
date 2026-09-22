# BUPTapi-dsh — One-Click BUPT DSH Setup

[简体中文](README.md) · [Security policy](SECURITY.md)

A DSH one-click access toolkit for the Beijing University of Posts and Telecommunications (BUPT) campus-network environment, with configuration and installer packages for Windows and macOS. Unzip the archive and double-click the corresponding entry point; enter the BUPT API Key issued by the university, and the tool writes the DSH configuration and local credential, starts DSH Web, and opens the page in your browser. Current version: **1.0.1**.

## Features and boundaries

- One-click configuration (`bupt-dsh-setup.exe`): writes `settings.yaml` and `.credentials.yaml` automatically; no manual config editing.
- One-click install (`dsh-download-setup.exe`): downloads and installs Node.js v24.19.0 and DSH (`@deepseek-ai/dsh`) automatically, without administrator rights; reuses an existing installation when detected.
- Official sources: Node.js is fetched from the official nodejs.org distribution and verified against the official `SHASUMS256.txt`; DSH is installed from the official npm registry.
- Private input: the BUPT API Key is not echoed while typing and is never written into the archive, source code, or normal logs.
- Automatic backup: existing configuration files are backed up with a timestamp (`.bupt-backup-<timestamp>`) before writing.
- Configuration merge: existing official or other Provider routes are preserved; the BUPT campus model is appended, with the previous configuration backed up before changing the default model.
- macOS support: `.command` one-click entry points support both Apple Silicon (arm64) and Intel (x86_64).
- Endpoint probe: the BUPT endpoint is probed before launch (12 s timeout); if unreachable, a hint is printed and DSH still starts.
- Automatic launch: finds `dsh.cmd`, runs `dsh web --port 3080`, waits for the port, then opens `http://127.0.0.1:3080`.
- Quick relaunch: after first-time configuration, double-click `start-dsh.cmd` (Windows) or `start-dsh.command` (macOS) to start DSH without entering the API Key again.
- Updating the key: run the configuration EXE again to update the Key and restart DSH; pass `--config-only` to only write the configuration.
- Boundaries: API Key issuance, account registration, and billing are out of scope.

## What is in the repository

| File | Description |
| --- | --- |
| `dsh-install.zip` | One-click install archive (v1.0.1, 61.23 MB): installs DSH automatically, then guides you through configuration; for fresh machines. SHA-256: `fcc3130b606f341e7402d673cd38e548a1bfc2ab294ae0209a69bb91ae10c38f` |
| `bupt-dsh-setup.zip` | Windows one-click configuration archive (v1.0.1, 30.61 MB): for environments where DSH is already installed. SHA-256: `bbc9588c25688f8bad957f1ad779218565191213ed90ecb75fba4cd97d4d7ab` |
| `dsh-install-mac.zip` | macOS one-click installer: downloads Node.js, installs DSH, then completes configuration. SHA-256: `858b97486dcf11482225138f8dd8b53578a8fa7ad8d400f0ddee336187d649ef` |
| `bupt-dsh-setup-mac.zip` | macOS one-click configuration archive for systems with Node.js/DSH already installed. SHA-256: `39a2cc8f48b4f724ad7cf1f42c3ff0bf392fe0c198d42eaddb1092f91b68faa6` |
| `README.md` | Chinese documentation |
| `README.en.md` | English documentation |
| `SECURITY.md` | Security policy |

Contents of the archives:

```text
dsh-install.zip                    bupt-dsh-setup.zip
├── dsh-download-setup.exe        ├── bupt-dsh-setup.exe
├── bupt-dsh-setup.exe            ├── start-dsh.cmd
├── start-dsh.cmd                  └── README.txt
└── README.txt

dsh-install-mac.zip                bupt-dsh-setup-mac.zip
├── dsh-download-setup.command     ├── bupt-dsh-setup.command
├── start-dsh.command              ├── start-dsh.command
├── configure.mjs                  ├── configure.mjs
└── README.txt                     └── README.txt
```

## Requirements

- Windows 10 / 11 (verified on Windows 11), 64-bit.
- macOS 12 or later; Apple Silicon and Intel are supported.
- `dsh-install.zip`: nothing needs to be pre-installed; internet access is required (about 50–80 MB of downloads), no administrator rights needed.
- `bupt-dsh-setup.zip`: DSH / Node.js must already be installed, with `dsh` available from the command line (i.e. `dsh.cmd` resolvable).
- BUPT campus network; connect to the campus VPN before use outside campus.

## Install and use

Quick start for new students (recommended: `dsh-install.zip`):

1. Download `dsh-install.zip` and unzip it into any folder.
2. Double-click `dsh-download-setup.exe` and wait for the automatic install (about 3–10 minutes depending on your network), no administrator rights needed.
3. When the install finishes, type `Y` when prompted to enter the configuration flow; enter the BUPT API Key issued by the university (characters are not displayed while typing).
4. Wait for the browser to open the DSH page (`http://127.0.0.1:3080`).
5. Later, double-click `start-dsh.cmd` in the same folder to relaunch DSH without entering the API Key again.

For macOS users:

1. Without DSH / Node.js: download `dsh-install-mac.zip`, unzip it, and double-click `dsh-download-setup.command`.
2. With DSH / Node.js already installed: download `bupt-dsh-setup-mac.zip`, unzip it, and double-click `bupt-dsh-setup.command`.
3. If macOS blocks the first run, right-click the `.command` file and choose “Open”; if needed, run `chmod +x *.command` in Terminal.
4. Enter the BUPT API Key and wait for the DSH page to open.
5. Later, double-click `start-dsh.command` in the same folder to relaunch DSH without entering the API Key again.

For users who already have DSH / Node.js:

1. Download `bupt-dsh-setup.zip` and unzip it into any folder.
2. Double-click `bupt-dsh-setup.exe`.
3. Enter the BUPT API Key issued by the university (characters are not displayed while typing).
4. Wait for the browser to open the DSH page (`http://127.0.0.1:3080`).
5. Later, double-click `start-dsh.cmd` in the same folder to relaunch DSH without entering the API Key again.

Write the configuration only, without launching:

```text
bupt-dsh-setup.exe --config-only
```

- “北邮接口暂不可达” (BUPT endpoint unreachable): confirm the campus VPN is connected; DSH will still start.
- “未找到 dsh.cmd” (dsh.cmd not found): run `dsh-download-setup.exe` first to complete the install, or install DSH / Node.js manually and retry.
- “DSH 未在预期时间内打开（端口 3080）” (DSH did not open in time): check the errors shown in the DSH window.

## Configuration and data

| Item | Default or location |
| --- | --- |
| Configuration directory | `%DSH_HOME%` (falls back to `%USERPROFILE%\.dsh`) |
| Model configuration | `settings.yaml` (managed block for provider `bupt-campus`) |
| Local credential | `.credentials.yaml` (stores `BUPT_API_KEY`) |
| Backup of existing config | `settings.yaml.bupt-backup-<timestamp>`, `.credentials.yaml.bupt-backup-<timestamp>` |
| Model endpoint | `https://myai.bupt.edu.cn/llm-gw/v1`, model `deepseek-v4-flash` |
| Local page | `http://127.0.0.1:3080` |
| Install location | The one-click installer puts Node.js / DSH under the user directory `%LOCALAPPDATA%\Programs\nodejs` (contains `node.exe` and `dsh.cmd`) and appends that directory to the current user's `PATH` (deduplicated) |

## Security

- A BUPT API Key is an access credential issued by the university, equivalent to an account password. `.credentials.yaml` and its timestamped backups are sensitive files; never upload them, paste them into issues, or share them in debug packages.
- The installer inside `dsh-install.zip` only downloads Node.js from the official nodejs.org address (verified against the official `SHASUMS256.txt`) and installs DSH from the official npm registry; no third-party software is bundled and there is no telemetry.
- Off-campus use requires the campus VPN. Do not connect to campus services from untrusted networks such as public Wi-Fi.
- Verify the SHA-256 of release archives (see the table above).
- Report security issues through GitHub private vulnerability reporting. See the [security policy](SECURITY.md).

## Boundaries

- The tool depends on the DSH command-line environment, campus-network reachability, and campus-side endpoint status. If the endpoint or configuration layout changes, startup or model availability may fail and needs manual review.
- `dsh-download-setup.exe` needs access to nodejs.org and the npm registry; if outbound internet is blocked on campus, the install step will fail — connect to the internet (or a usable proxy) first.
- The tool only configures and launches; it does not issue API Keys, manage accounts, or recover deleted configurations.
- Previous configurations are kept as timestamped backups; `.bupt-backup-*` files can be removed manually when rollback is no longer needed.

## License

The repository currently has no open-source license. Public visibility is not an open-source license grant; confirm permitted use and redistribution with the maintainer.
