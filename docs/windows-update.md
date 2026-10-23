# Windows 自动更新修复

2026-10-23：V0.1.1 作为 GitHub Latest 提供 Windows x64 签名完整安装包。

## V0.1.0 检查不到新版本

部分旧客户端首先读取遗留下载站清单。清单中的版本不属于当前兼容版本线时，客户端可能提前结束检查并显示“已是最新版本”，没有继续访问 GitHub。更新清单无法远程改变旧程序里写入的检查地址。

请先结束设备任务、正常退出 LYNX，再使用以下任一路径：

1. 希望保留 V0.1.0 并验证自动更新：手动覆盖安装 [V0.1.0 更新引导修复包](https://github.com/dshanpi/lynx-releases/releases/download/v0.1.0/LYNX_0.1.0_github-update_x64-setup.exe)，启动后打开设置，点击“检查更新”，应发现 V0.1.1。
2. 直接使用新版：手动覆盖安装 [V0.1.1 更新修复包](https://github.com/dshanpi/lynx-releases/releases/download/v0.1.1/LYNX_0.1.1_github-update_x64-setup.exe)。

也可下载包含安装程序、签名、公钥和校验说明的 ZIP：

- [V0.1.0 更新引导 ZIP](https://github.com/dshanpi/lynx-releases/releases/download/v0.1.0/LYNX_0.1.0_Windows_x64_github-update.zip)
- [V0.1.1 更新修复 ZIP](https://github.com/dshanpi/lynx-releases/releases/download/v0.1.1/LYNX_0.1.1_Windows_x64_github-update.zip)

请认准文件名中的 `github-update`。同一发布页原有文件保留用于历史版本追溯，不代表包含本次更新源修复。覆盖安装前建议备份自己的重要数据。

## 更新方式

修复后的客户端只使用 GitHub 更新清单，不依赖 dl.100ask.net。软件仍会征求是否下载安装，使用应用中已有的公钥验证安装包。不同构建的 V0.1.0 统一使用完整安装包更新，避免精确增量基线不一致；不要手动运行历史增量包来替代此流程。

网络连接失败应显示具体错误。请确认 GitHub 及其发布文件下载域名可访问，必要时检查系统代理，然后重新检查更新。网络失败不表示已经是最新版本。

更新签名与 Windows Authenticode 发布者签名是不同机制，系统仍可能显示未知发布者。

## 验证范围

Ubuntu 中的真实 Tauri 更新器已经在网络重试后完成 V0.1.0 → V0.1.1 Windows 更新目标的发现、完整下载和既有公钥验签；同版本不重复更新、坏代理返回网络错误的检查也已通过。这不能替代原生 Windows 的安装、启动、覆盖安装、重启和卸载验收；这些步骤由隔离 Windows 测试机执行。未将此修复声明为所有 Windows 安装环节均已通过。

本次更新仅交付 Windows x64 修复包，不代表 Linux、macOS 或全部板卡和课程重新完成验收。

更新期间请保持设备任务停止。运行任务时的自动安装拦截不属于本轮已通过的验证，不能依靠普通窗口关闭保护来推断安装阶段安全。
