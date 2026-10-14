# ChatGPT 代理助手 v1.0.0

让 **ChatGPT 桌面版直接走本地代理启动**，从此不用再开 Windows 系统代理，也不用开 TUN 模式。

## ✨ 这个版本能做什么

- **一键开启**：写入用户级代理环境变量、创建「ChatGPT（本地代理）」快捷方式、接管桌面原「ChatGPT」快捷方式（自动备份），并按需立即启动 ChatGPT；配置动作 1 秒内完成
- **一键还原**：环境变量、快捷方式、原始快捷方式全部按记录恢复，系统回到安装前的状态
- **环境检测**：ChatGPT 版本与安装方式 / 代理端口占用进程 / 系统代理 / TUN / 环境变量 / 快捷方式 / 运行状态，共 7 项实时探测
- **真实连通性测试**：用 HTTP `CONNECT` 实际打通道 `chatgpt.com:443`，不是只探测端口
- **三档主题**：浅色（默认）/ 深色 / 跟随系统，实时跟随系统深浅色切换
- **原生质感界面**：无边框窗口 + Windows 11 Mica 材质 + 自绘标题栏与图标
- **命令行开关**：`--launch` / `--enable` / `--restore` / `--test` / `--detect`

## 📦 安装与使用

1. 下载本 Release 里的 `ChatGPTProxyManager.exe`（单文件，免安装，约 460 KB）
2. 双击运行 → 确认代理地址（默认 `127.0.0.1:7897`）→ 点「测试连通性」确认显示"可用"
3. 点「一键开启」，之后**直接双击桌面 ChatGPT 快捷方式**即可，无需系统代理或 TUN

> 首次运行 Windows SmartScreen 可能提示"未知发布者"（未做代码签名）：点「更多信息」→「仍要运行」。
> 介意的话可以 clone 源码用 `build\build.ps1` 自行编译，编译产物与本附件完全一致（同一份源码）。

## 🖥 系统要求

- Windows 10 19041+ / Windows 11（x64）
- .NET Framework 4.8（Win10 1903+ 与 Win11 **系统自带**）
- 一个正在运行的本地代理客户端（Clash Verge / mihomo / sing-box / v2rayN …，占本地 HTTP 混合端口即可）
- 已安装 ChatGPT 桌面版（Microsoft Store 的 MSIX 应用，包名 `OpenAI.Codex`）

## 🔍 为什么需要它（背景）

ChatGPT 桌面版是 MSIX 打包应用，内部有 Chromium 外壳 + Rust/Node 组件两套网络栈：
Chromium **只认系统代理或 `--proxy-server` 启动参数，不认 `HTTP_PROXY` 环境变量**；
而 MSIX 应用的快捷方式不允许附加启动参数。所以只设环境变量时，界面仍会卡在启动画面——
这就是"必须去开系统代理 / TUN"的根本原因。

本工具用 COM 激活接口 `IApplicationActivationManager::ActivateApplication` 带参数启动应用
（保留应用标识，登录/通知/自动更新不受影响），再补齐环境变量覆盖另一半网络栈，
并把快捷方式指向自身作为启动器 —— 于是"双击 ChatGPT"就等于"带代理启动 ChatGPT"。

## 🔐 安全与可逆性

- 全程**不需要管理员权限**，不装服务、不装驱动、不改动代理客户端配置
- 只写：用户级环境变量、桌面/开始菜单快捷方式（均有还原路径）
- 系统代理与 TUN **始终是关闭状态**（程序界面里可直接看到检测结果）
- 卸载：点「一键还原」后删除 `%LOCALAPPDATA%\ChatGPTProxyManager` 即可

## 📄 文件校验

```
SHA256  ChatGPTProxyManager.exe
f3e3ac562ab13755d298090dc7165ef8689e237f767a0949baa4f07b38258f9c
```

## 许可

MIT · 本项目与 OpenAI 无任何关联，仅使用其公开安装的桌面应用。

---
---

# ChatGPT Proxy Assistant v1.0.0 (English)

Launch the **ChatGPT desktop app** through your local proxy — **no Windows system proxy, no TUN mode**.

## ✨ What's in this release

- **One-click enable** — writes the user-level proxy environment variables, creates a "ChatGPT (Local Proxy)" shortcut, takes over the desktop ChatGPT shortcut (original backed up) and optionally launches the app; the configuration itself takes under a second
- **One-click restore** — removes the environment variables, deletes the created shortcuts and restores the original one, returning the system to its pre-install state
- **Environment detection** — 7 live checks: ChatGPT version & install type, proxy-port owner, system proxy, TUN, environment variables, shortcuts, running state
- **Real connectivity test** — performs an HTTP `CONNECT` to `chatgpt.com:443` instead of merely probing the port
- **Three themes** — light (default) / dark / follow system, live-following Windows theme changes
- **Native-feeling UI** — borderless window with Windows 11 Mica material, custom title bar and icon
- **CLI switches** — `--launch` / `--enable` / `--restore` / `--test` / `--detect`

## 📦 Install & use

1. Download `ChatGPTProxyManager.exe` from this release (single file, ~460 KB, no installer)
2. Run it, confirm the proxy address (default `127.0.0.1:7897`) and press **Test connectivity** until it reports "available"
3. Press **One-click enable** — from then on just double-click the desktop **ChatGPT** shortcut, no system proxy or TUN needed

> Windows SmartScreen may warn about an unknown publisher (the exe is not code-signed): choose
> "More info" → "Run anyway". You can also build it yourself from source with `build\build.ps1`; the build
> is reproducible from this repository and matches this attachment (same source tree).

## 🖥 Requirements

- Windows 10 19041+ / Windows 11 (x64)
- .NET Framework 4.8 — **included with** Windows 10 1903+ and Windows 11
- A running local proxy client (Clash Verge / mihomo / sing-box / v2rayN …) listening on a local HTTP/mixed port
- The ChatGPT desktop app installed (Microsoft Store MSIX package `OpenAI.Codex`)

## 🔍 Why it's needed

The app is an MSIX package with two network stacks: its Chromium shell **only honours the Windows system
proxy or a `--proxy-server` launch switch and ignores `HTTP_PROXY`**, while MSIX shortcuts cannot carry
arguments — so environment variables alone leave the UI stuck on the splash screen. This tool activates the
packaged app through `IApplicationActivationManager::ActivateApplication` with `--proxy-server` (package
identity preserved, so sign-in / notifications / auto-update keep working), sets the user-level proxy
environment variables for the bundled Rust/Node parts, and repoints the ChatGPT shortcuts at itself as the
launcher — so "double-click ChatGPT" equals "launch ChatGPT proxied".

## 🔐 Safety & reversibility

- **No administrator rights**, no service, no driver, no changes to your proxy client's configuration
- Writes only user-level environment variables and desktop/Start-Menu shortcuts — both with a revert path
- System proxy and TUN stay **disabled** (visible in the detection panel)
- Uninstall: press **One-click restore**, then delete `%LOCALAPPDATA%\ChatGPTProxyManager`

## 📄 Checksum

```
SHA256  ChatGPTProxyManager.exe
f3e3ac562ab13755d298090dc7165ef8689e237f767a0949baa4f07b38258f9c
```

## License

MIT · Not affiliated with OpenAI in any way; it only drives the publicly installed desktop app.
