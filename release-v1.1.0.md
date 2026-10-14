## What's Changed

### Features
- `2e7520d` feat: 新增 macOS 版（SwiftUI 原生应用，open --args --proxy-server 方案）

### Documentation
- `df18239` docs: README 双平台化，新增 macOS 版介绍、截图与已知限制

## v1.1.0 · 新增 macOS 版

在 Windows 版基础上，新增 **macOS 原生版本**（SwiftUI，零第三方依赖），界面与功能保持一致：
一键开启 / 还原、7 项环境检测、真实连通性测试、代理地址可改（HTTP / SOCKS5）、三档主题、运行日志、命令行开关。

### macOS 版实现原理

mac 版 ChatGPT（bundle id `com.openai.codex`）与 Windows 版同源，**内核同样是 Chromium**。
macOS 的 `open` 命令原生支持透传启动参数与环境变量，因此无需 COM 激活、无需写注册表：

```bash
open /Applications/ChatGPT.app --args --proxy-server=http://127.0.0.1:7897
```

不修改系统代理、不创建 TUN 网卡；可选创建「ChatGPT（代理）」启动器到「应用程序」（复用 ChatGPT
官方图标），双击即得以代理方式启动，原应用零改动，一键还原即清空。

### 下载

- **Windows**：`ChatGPTProxyManager.exe` —— 与 v1.0.0 相同，无改动，可继续使用旧版
- **macOS**：`ChatGPT-Proxy-Helper-macos-arm64-1.1.0.zip`（Apple Silicon；Intel 机器请在 `macos/` 目录下从源码构建）

```
SHA-256: 682985d31320a87805e4ebd48880ffd52f9db4138a146b61f5f11c8ac2d86f9f
```

### macOS 安装说明

1. 解压后将「ChatGPT 代理助手.app」拖入「应用程序」；
2. 未做代码签名 / 公证，首次打开若提示"无法验证开发者"，在终端执行后重新打开：

   ```bash
   xattr -cr "/Applications/ChatGPT 代理助手.app"
   ```

3. 打开后确认代理地址 → 「测试连通性」→「一键开启」。

### 已知限制（macOS）

- ChatGPT 的「检查更新」由内置 Sparkle 框架走系统网络，不认 `--proxy-server` 也不认环境变量 ——
  需要更新 ChatGPT 时请临时开启系统代理，更新完成后再关闭（助手界面有「工具边界」提示）；
- 语音通话走 UDP，HTTP 代理无法转发此类流量。

**Full Changelog**: https://github.com/xztsummer/chatgpt-proxy-manager/compare/v1.0.0...v1.1.0
