<div align="center">
  <img width="112" src="https://raw.githubusercontent.com/dshanpi/lynx/main/assets/branding/lynx-icon-master.png" alt="DshanPI LYNX logo">
  <h1>DshanPI LYNX｜芯桥</h1>
  <p><strong>v0.1.0 正式版准备中 · 面向多芯片开发板的烧录、终端、远端 SDK 与 AI 调试工作台</strong></p>
  <p><strong>面向嵌入式开发板的 AI 工作台、跨芯片烧录器与远端 SDK 通道管理工具</strong></p>
  <p>
    <a href="https://github.com/dshanpi/lynx/actions/workflows/build.yml"><img src="https://github.com/dshanpi/lynx/actions/workflows/build.yml/badge.svg" alt="Build"></a>
    <a href="https://github.com/dshanpi/lynx-releases/releases"><img src="https://img.shields.io/badge/release-v0.1.0%20preparing-orange" alt="Release"></a>
    <a href="LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="Apache-2.0"></a>
  </p>
</div>

LYNX 把开发板管理、调试终端、固件烧录、远端 SDK 和 AI 编程工具放进同一个桌面应用。每块真实单板都有独立配置；每条 SDK 通道通过本机 MCP Gateway 和 SSH 反向隧道连接到远端 Linux 主机，让 Codex 或 Claude Code 能在明确的单板作用域内调用硬件能力。

当前状态：**v0.1.0 正式版准备中，尚未发布安装包。**

历史 GitHub Releases、安装包及版本标签已清理，后续正式版本从 `v0.1.0` 开始发布。

## 核心能力

### 多芯片单板管理

- 新建单板时选择芯片厂家，并绑定独立的串口、USB 烧录接口和 ADB 接口。
- 支持多块板同时保存，避免串口、USB 和 ADB 绑定跨板混用。
- 单板状态集中显示，包括调试串口、烧录接口、ADB 和最近烧录结果。
- 删除单板时自动停止并删除其 SDK 通道；共享 SSH 主机和凭证不会被误删。

### 调试与烧录

- 内置串口终端和 ADB 终端，按单板保存运行状态。
- 固件烧录进度、日志和任务历史统一展示。
- 烧录参数通过数组直接传递给 CLI，不经过 Shell。
- 每种芯片使用独立烧录适配器：

| 芯片厂家          | 工具                | 当前能力                                                     |
| ----------------- | ------------------- | ------------------------------------------------------------ |
| Allwinner 全志    | OpenixCLI / libefex | FEL/FES 扫描与固件烧录                                       |
| Rockchip 瑞芯微   | OpenRockCLI         | Maskrom/Loader 扫描、Rockusb 命令及跨平台 `update.img` 烧录  |
| NXP i.MX          | UUU                 | SDP/Fastboot 设备扫描、UUU 脚本及固件执行，自动绑定 USB 路径 |
| 嘉楠 K230/K230D   | LYNX 内置协议       | BootROM/Loader 扫描；EMMC、SD、SPI NAND、SPI NOR 与 OTP 烧录 |
| ST STM32MP1/STM32 | dfu-util            | USB DFU/DfuSe 扫描；按 USB 路径和 alternate setting 定向烧录 |

Allwinner 和 Rockchip 默认使用 `full_erase` 与烧录校验，降低旧文件系统状态影响启动的风险。
AI/MCP 烧录仅接受 `full_erase`，避免调用方显式传入 `partition` 绕过安全默认值。
NXP 严格执行单板保存的 UUU 脚本，不臆造通用擦除参数；K230 内置链路按镜像目标擦除后写入；
STM32MP1/STM32 由 DfuSe target、alternate setting 和地址决定擦写范围。

固件下载任务属于 LYNX 应用进程，不绑定创建它的 MCP session；Claude 重连或换 session 后仍可用同一
`taskId` 轮询。若 LYNX 进程自身重启，重新调用下载会从保留的 `.part` 文件续传并重新执行 SHA-256 校验。

进入 FEL/FES、Maskrom/Loader、SDP/Fastboot、BROM/U-Boot 或 USB DFU/DfuSe，只允许使用
物理启动按键/拨码，或后续明确配置的可靠硬件复位/电源控制。LYNX MCP 会拒绝
`syscon unbind`、`reboot-mode`、`devmem` 和软件 reboot-to-programming-mode 命令。

### 远端 SDK 与 AI 通道

- 每块单板可创建多条 SDK 通道。
- 相同的 SSH 地址、端口和用户名会复用同一主机及系统密钥环凭证。
- 不同 SDK 自动分配独立的本机 Gateway 端口和远端 MCP 端口。
- 多名研发连接同一台远端主机时，若保存的反向端口已被其他 LYNX 占用，SSH 服务端会原子分配空闲端口；实际端口自动保存并同步到启动命令。
- MCP 名称由 SDK 通道名称自动生成，例如：

```text
T153-OminiGate SDK → lynx-t153-ominigate-sdk
```

- 自动启动本机 MCP Gateway、SSH 反向隧道并执行远端健康检查。
- 支持 Codex 和 Claude Code；启动命令以内联配置仅挂载当前 SDK 通道，不读取或修改共享 SDK 目录及用户级 MCP 配置。
- SSH 主机指纹固定，凭证保存在系统安全存储中，不写入 SQLite 配置数据库。

## 系统要求

| 平台        | 支持状态 | 说明                                                                       |
| ----------- | -------- | -------------------------------------------------------------------------- |
| Windows x64 | 支持     | 提供 NSIS 和 MSI 安装包；烧录 CLI 及其运行库随应用打包                     |
| Linux x64   | 支持     | 提供 AppImage、DEB 和 RPM；Rockchip `update.img` 整包烧录使用 Linux 工具链 |
| macOS ARM64 | 支持     | 提供 DMG 和 `.app`；当前构建未进行 Apple 公证                              |

USB、串口和 ADB 能否访问还取决于操作系统驱动、设备权限及开发板当前启动模式。

## 安装

目前暂无可下载的正式安装包。完成构建、签名和安装验收后，将在 [LYNX Releases](https://github.com/dshanpi/lynx-releases/releases) 发布 `v0.1.0`。

正式发布时，发布页将提供：

- Windows、Linux、macOS 安装包；
- Tauri 自动更新签名；
- `latest.json` 更新清单；
- `SHA256SUMS.txt` 文件校验和。

建议在安装前核对 SHA-256。Linux 的 DEB/RPM 安装包会安装嘉楠 K230 udev 规则，安装后重新插拔即可由普通用户烧录；AppImage 用户或其他 USB/串口设备仍应检查 udev 规则及用户组权限。Windows 用户应确认开发板 USB 驱动安装正确。

## 快速开始

1. 打开 LYNX，进入“单板工作台”。
2. 点击“新建”，选择芯片厂家并填写单板名称。
3. 扫描并绑定该板的串口、USB 烧录接口和 ADB 接口。
4. 保存单板。
5. 添加 SDK 通道，填写远端主机地址、SSH 用户和 SDK 信息。
6. 首次连接时测试并确认 SSH 主机指纹；如需密码登录，输入一次密码并保存到系统密钥环。
7. 保存通道后，LYNX 会自动启动 Gateway 和隧道。
8. 复制界面生成的 Codex 或 Claude Code 启动命令，在目标 SDK 目录启动 AI 工具。

同一台远端主机可以服务多块开发板和多个 SDK。LYNX 会自动处理端口及 MCP 名称冲突，不需要重复保存 SSH 凭证。

## 架构

```text
┌───────────────────────────────────────────────────────────────┐
│                    LYNX Desktop (Tauri)                       │
│                                                               │
│  React 工作台 ── Tauri IPC ── Rust 任务、设备与安全边界       │
│       │                         │                              │
│       │                         ├─ Serial / ADB                │
│       │                         ├─ OpenixCLI / OpenRockCLI     │
│       │                         ├─ NXP UUU                     │
│       │                         ├─ K230 native USB protocol    │
│       │                         └─ STM32 dfu-util              │
│       │                         └─ SQLite + 系统密钥环          │
│       │                                                        │
│       └─ MCP Gateway (127.0.0.1:8765+)                         │
│                         │                                      │
└─────────────────────────┼──────────────────────────────────────┘
                          │ SSH reverse tunnel
                          ▼
                远端 Linux SDK 主机
                127.0.0.1:动态端口/mcp
                          │
                          ▼
                 Codex / Claude Code
```

### 安全边界

- 烧录 CLI 参数不经过 Shell。
- SSH 连接使用固定主机指纹，主机密钥变化会中止连接。
- 密码和私钥口令存放于 Windows Credential Manager、macOS Keychain 或 Linux Secret Service。
- SQLite 只保存非敏感配置和任务历史。
- 每条 MCP Gateway 都携带单板、项目和端口作用域，防止多板路由串线。
