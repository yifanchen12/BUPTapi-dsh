# BUPTapi-dsh

北邮 DSH 一键接入工具包（BUPT DSH Setup）。

## 内容

- `bupt-dsh-setup.zip`：一键配置启动包（解压后得到 `bupt-dsh-setup.exe`）。

## 使用前

1. 安装 DSH / Node.js，并确认命令行中能运行 `dsh`。
2. 校外使用时先连接北京邮电大学校园 VPN。

## 使用

1. 解压 `bupt-dsh-setup.zip`，双击 `bupt-dsh-setup.exe`。
2. 输入学校发的 BUPT API Key；输入时不会显示字符。
3. 等待浏览器自动打开 DSH 页面。

## 说明

- 这个 EXE 不包含 DSH 本体，只负责配置和启动你电脑上已有的 DSH。
- 配置写入当前用户的 `%USERPROFILE%\.dsh`；原配置会自动生成带时间的备份。
- API Key 不会写入压缩包、源码或普通日志。
- 如果提示接口不可达，请确认校园 VPN；如果提示找不到 `dsh.cmd`，请先安装 DSH / Node.js。
