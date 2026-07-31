<div align="center">
  <img width="112" src="https://raw.githubusercontent.com/dshanpi/lynx/main/assets/branding/lynx-icon-master.png" alt="DshanPI LYNX logo">
  <h1>DshanPI LYNX｜芯桥</h1>
  <p><strong>内测版 · 面向多芯片开发板的烧录、终端、远端 SDK 与 AI 调试工作台</strong></p>
  <p>
    <a href="https://github.com/dshanpi/lynx-releases/releases/latest"><img src="https://img.shields.io/github/v/release/dshanpi/lynx-releases" alt="Latest release"></a>
    <a href="https://github.com/dshanpi/lynx/actions/workflows/build.yml"><img src="https://github.com/dshanpi/lynx/actions/workflows/build.yml/badge.svg" alt="Build"></a>
    <a href="https://github.com/dshanpi/lynx"><img src="https://img.shields.io/badge/source-dshanpi%2Flynx-blue" alt="Source repository"></a>
  </p>
</div>

本仓库是 **DshanPI LYNX｜芯桥的官方安装包与自动更新仓库**，仅保存经过 GitHub Actions 构建的 Windows、Linux、macOS 安装包、Tauri 更新签名和 SHA-256 校验文件。

软件源代码、问题反馈和开发文档位于 [`dshanpi/lynx`](https://github.com/dshanpi/lynx)。

> [!WARNING]
> 当前版本属于内测版，功能、配置格式和界面仍可能调整。请勿将它作为无人值守量产环境中的唯一烧录工具；执行重要烧录操作前请备份固件与设备数据。

## 下载

前往 [最新版本下载页](https://github.com/dshanpi/lynx-releases/releases/latest)。

### 应该下载哪个文件

| 操作系统 | 推荐文件 | 用途 |
| --- | --- | --- |
| Windows 10/11 x64 | `LYNX_*_x64-setup.exe` | 推荐给普通用户的 NSIS 安装程序 |
| Windows 10/11 x64 | `LYNX_*_x64_en-US.msi` | 企业部署、批量安装或 MSI 管理 |
| Ubuntu / Debian x64 | `LYNX_*_amd64.deb` | Debian、Ubuntu 及其衍生发行版 |
| Fedora / RHEL x64 | `LYNX-*.x86_64.rpm` | Fedora、RHEL、Rocky Linux 等 |
| 通用 Linux x64 | `LYNX_*_amd64.AppImage` | 无需安装，授权执行后直接运行 |
| macOS Apple Silicon | `LYNX_*_aarch64.dmg` | M1、M2、M3、M4 等 ARM64 Mac |

`.sig` 文件用于应用内自动更新签名验证，普通手动安装一般不需要单独下载。`latest.json` 是自动更新清单，`SHA256SUMS.txt` 用于人工核对下载文件。

## 软件功能

LYNX 将多块开发板、固件烧录、串口和 ADB 终端、远端 SDK、SSH 隧道与 AI 编程工具放在同一个桌面工作台中。

### 多芯片单板管理

- 新建单板时先选择芯片厂家，再填写单板名称。
- 每块单板独立绑定串口、USB 烧录接口和 ADB 设备。
- 多块板可以同时保存，避免串口或 USB 接口被不同单板错误复用。
- 单板设置页底部可以删除当前单板；其 SDK 通道会同步删除，共享 SSH 主机和凭证会保留。

### 烧录支持

| 芯片厂家 | 内置工具 | 当前支持 |
| --- | --- | --- |
| Allwinner 全志 | OpenixCLI / libefex | FEL/FES 扫描与固件烧录 |
| Rockchip 瑞芯微 | OpenRockCLI | Maskrom/Loader 扫描、Rockusb 命令；Linux 支持 `update.img` 流程 |
| NXP i.MX | UUU | SDP/Fastboot 扫描、UUU 脚本和固件执行、USB 路径绑定 |
| 嘉楠 K230/K230D | LYNX 内置协议 | BootROM/Loader 扫描；EMMC、SD、SPI NAND、SPI NOR 与 OTP 烧录 |
| ST STM32MP1 | 规划中 | DFU 适配规划中，暂未启用 |

烧录 CLI 和所需动态库随安装包一起发布。K230 协议及对应 Loader 已直接内置，无需填写外部 CLI 路径或烧录参数。Windows 安装包会自动解包验证 `libusb-1.0.dll`、OpenixCLI、OpenRockCLI 和 UUU，正常情况下不需要手工复制 DLL。

### 远端 SDK 与 AI 通道

- 每块单板可以建立独立的 SDK 通道。
- 相同 SSH 地址、端口和用户名会复用主机及安全凭证，不会反复索要密码。
- 不同 SDK 自动分配独立的本机 Gateway 端口、远端 MCP 端口和 MCP 名称。
- 配置保存后，LYNX 自动启动 MCP Gateway、SSH 反向隧道并刷新 AI 通道。
- 支持生成 Codex 和 Claude Code 的专属启动命令，只挂载当前 SDK 通道，不修改用户级全局 MCP 配置。

## 安装说明

### Windows

1. 下载 `LYNX_*_x64-setup.exe`。
2. 双击安装并按向导完成操作。
3. 如果 Windows SmartScreen 提示来源未知，请先核对下载地址和 SHA-256，再选择继续运行。
4. 连接开发板后安装对应厂家的 USB 驱动；驱动类型取决于芯片启动模式。

若更适合集中部署，可以使用 MSI：

```powershell
msiexec /i LYNX_0.1.2_x64_en-US.msi
```

### Ubuntu / Debian

```bash
sudo apt install ./LYNX_0.1.2_amd64.deb
```

如果无法访问 USB 或串口，请检查 udev 规则以及当前用户的 `dialout`、`plugdev` 等设备访问权限。

### Fedora / RHEL

```bash
sudo dnf install ./LYNX-0.1.2-1.x86_64.rpm
```

### AppImage

```bash
chmod +x LYNX_0.1.2_amd64.AppImage
./LYNX_0.1.2_amd64.AppImage
```

### macOS ARM64

1. 下载并打开 `LYNX_*_aarch64.dmg`。
2. 将 LYNX 拖入 Applications。
3. 当前内测包尚未进行 Apple 公证；首次启动若被 Gatekeeper 阻止，请在“系统设置 → 隐私与安全性”中确认来源后手动允许。

目前只提供 Apple Silicon ARM64 构建，不支持 Intel Mac。

## 第一次使用

### 1. 新建单板

1. 打开“开发工作台”。
2. 点击新建单板。
3. 在左侧选择芯片厂家。
4. 填写单板名称。
5. 扫描并绑定串口、烧录 USB 接口和 ADB 设备。
6. 保存单板。

同一物理接口不能同时绑定给多块单板。开发板重新枚举后，LYNX 会根据保存的 USB 信息尽量恢复绑定。

### 2. 烧录固件

1. 选择目标单板。
2. 确认开发板已经进入对应烧录模式：
   - Rockchip：Maskrom 或 Loader；
   - NXP i.MX：SDP、SDPU、SDPV 或 Fastboot；
   - 嘉楠 K230：BootROM 或已下载 Loader 的阶段；
   - Allwinner：FEL/FES。
3. K230 单板还需在设置中选择 EMMC、SD Card、SPI NAND、SPI NOR 或 OTP 目标介质。
4. 在工作台选择固件或烧录任务。
5. 启动烧录并观察进度、输出及任务历史。

烧录过程中不要拔掉 USB、电源或关闭应用。烧录失败时先保存完整日志，再检查驱动、线材、供电和芯片启动模式。

### 3. 配置远端 SDK

1. 在目标单板中添加 SDK 通道。
2. 填写通道名称、主机名称、IP 或域名、SSH 用户和端口。
3. 填写远端 SDK 目录及需要的构建、下载命令。
4. 首次连接时核对 SSH 主机指纹。
5. 如使用密码登录，只需输入一次；凭证会写入操作系统安全存储。
6. 保存后等待 Gateway 和 SSH 隧道自动连接。
7. 复制界面生成的 Codex 或 Claude Code 命令，在目标 SDK 目录启动 AI 工具。

多块板可以连接同一台 SSH 主机。LYNX 会复用相同 SSH 身份，但仍为每块板、每个 SDK 保留独立通道和端口。

## 自动更新

客户端从本仓库的 `latest.json` 检查更新，并使用内置公钥验证 Tauri 更新签名。自动更新不会信任未签名或签名不匹配的安装包。

由于早期公开版本曾使用较大的实验性版本号，版本号降低时客户端可能把新版视为“降级”。遇到这种情况，请从本仓库手动下载并覆盖安装。

## 校验下载文件

每个 Release 都包含 `SHA256SUMS.txt`。

Linux：

```bash
sha256sum -c SHA256SUMS.txt
```

Windows PowerShell：

```powershell
Get-FileHash .\LYNX_0.1.2_x64-setup.exe -Algorithm SHA256
```

将输出与 `SHA256SUMS.txt` 中对应文件的摘要进行比较。

## 常见问题

### Windows 提示找不到 `libusb-1.0.dll`

正式安装包已经内置并验证该 DLL。请确认使用的是本仓库最新的 EXE 或 MSI，不要只复制单独的主程序；如果安全软件隔离了 DLL，请查看隔离记录并重新安装。

### 扫描不到 Rockchip 开发板

- 确认设备处于 Maskrom 或 Loader 模式。
- 更换支持数据传输的 USB 线和稳定 USB 端口。
- 检查 Windows 设备管理器中的 Rockusb 驱动。
- 避免让其他烧录工具同时占用设备。

### NXP UUU 扫描不到设备

- 检查芯片是否进入 SDP/Fastboot 模式。
- 确认 USB VID/PID 与 UUU 支持列表匹配。
- 关闭正在占用该 USB 设备的其他 UUU 进程。

### 嘉楠 K230 扫描不到设备

- 确认芯片已进入 USB BootROM 或 Loader 模式，USB VID/PID 应为 `29f1:0230`。
- Windows 需要为设备安装可由 libusb 访问的驱动；Linux 需要相应 udev 权限。
- 关闭其他 K230 烧录工具，避免它们占用 USB interface 0。
- 烧录前务必核对目标介质，尤其是 OTP；OTP 写入具有不可逆风险。

### 为什么系统要求解锁密钥环

LYNX 使用操作系统密钥环保存 SSH 密码或私钥口令，避免把明文凭证写入配置数据库。密钥环锁定时，系统可能要求解锁一次；相同 SSH 身份随后会复用凭证。

### SDK 通道端口冲突

保存通道时，LYNX 会自动选择下一组可用的 Gateway、远端 MCP 端口和 MCP 名称。相同主机不等于相同 SDK，多块板和多个 SDK 可以分别保存。

## 安全与反馈

- 安装包由 [`dshanpi/lynx`](https://github.com/dshanpi/lynx) 的 GitHub Actions 自动构建。
- 发布过程校验内置 CLI、运行时、动态库、安装包内容和更新签名。
- SSH 主机指纹需要用户首次确认。
- SSH 凭证保存在系统安全存储中。

发现问题时，请前往 [`dshanpi/lynx/issues`](https://github.com/dshanpi/lynx/issues) 提交，并附上：

- 操作系统和 LYNX 版本；
- 芯片型号与开发板名称；
- USB 启动模式；
- 完整错误日志；
- 可复现步骤。

## 仓库分工

| 仓库 | 内容 |
| --- | --- |
| [`dshanpi/lynx`](https://github.com/dshanpi/lynx) | 应用源代码、开发文档、Issues、GitHub Actions |
| [`dshanpi/lynx-releases`](https://github.com/dshanpi/lynx-releases) | 正式安装包、更新签名、`latest.json`、SHA-256 校验 |
