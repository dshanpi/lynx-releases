# 功能演示

本页收录实际界面的截图和操作步骤动图。含模拟课程、设备或结果的画面明确标记。演示用于使用说明，不替代线上、硬件、安装或成绩验收。

| 主题 | 使用方法 | 数据范围 |
| --- | --- | --- |
| 应用导航 | 工作台、学习中心与 AI 配置切换 | 独立本地原生实例 |
| AI 辅助课程 | 列表、详情、章节与进入学习 | 隔离模拟数据 |
| 课程阅读 | 导航、正文、指引和滚动 | 隔离模拟数据 |
| 复盘保持 | 输入、切章、返回后草稿仍保留 | 隔离模拟数据；不演示正式保存或成绩 |
| AI 配置 | 服务字段、运行参数、侧栏变化 | 无真实密钥，不保存模拟连接 |

素材来自 macOS 上实际运行的 Tauri 窗口。GIF/MP4 由操作后的原始窗口截图按每步约 3 秒合成，**不是连续录屏**，无旁白。窗口中的“主分支整合验证”和“模拟数据演示”标识保留，不冒充已发布安装包。

## 1. 应用导航

MCU 工作台 → 学习指南 → 实验中心 → AI 配置 → 创建任务。使用独立、未登录的本地实例；空状态用于展示首次使用路径，不代表账号已有课程或设备。

![应用导航](../media/gif/navigation.gif)

[观看 MP4](../media/video/navigation.mp4)

## 2. AI 配置与策略

填写演示连接名称，查看 API 地址和模型字段，展开运行预算，再查看策略。`example.invalid` 为不可用的示例地址；没有输入真实密钥，没有保存或发送连接测试。

![AI 配置与策略](../media/gif/ai-configuration.gif)

[观看 MP4](../media/video/ai-configuration.mp4)

## 3. 学习工作区与草稿保持

进入模拟课程，收起/展开右侧指引，输入复盘，切换章节后返回。草稿仍属于原章节；离开课程前需要保存。

该示例只有模拟课程读取与学习记录接口，**没有正式评测服务或 AI 服务**。画面保留连接检查失败及评测按钮不可用状态。演示不能证明正式课程同步、评测、报告、SSH、构建或硬件闭环通过。

![学习工作区与草稿保持](../media/gif/learning-demo.gif)

[观看 MP4](../media/video/learning-demo.mp4)

## 高清截图

点击图片可查看原始尺寸。

| 界面 | 截图 |
| --- | --- |
| 学习指南（模拟） | [查看](../media/screenshots/learning-guide-demo.png) |
| 课程工作区（模拟） | [查看](../media/screenshots/course-workspace-demo.png) |
| 复盘草稿（模拟） | [查看](../media/screenshots/reflection-draft-demo.png) |
| MCU 工作台空状态 | [查看](../media/screenshots/mcu-workbench.png) |
| AI 服务 | [查看](../media/screenshots/ai-service.png) |
| AI 运行限制 | [查看](../media/screenshots/ai-limits.png) |
| AI 策略 | [查看](../media/screenshots/ai-policy.png) |
| 任务编排入口 | [查看](../media/screenshots/task-builder.png) |

媒体文件 SHA-256 见 [校验清单](../media/SHA256SUMS.txt)。演示适配器、测试脚本和产品源码不在本仓库中。
