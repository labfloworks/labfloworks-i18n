---
title: FloWorks
description: 通用可视化实验室，用于信号处理、科学仪器和自动化。
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## 信号、仪器和人工智能的通用可视化实验室

科学处理 • 数字信号处理 • VISA/SCPI • 自动化 • 机器学习

![FloWorks 截图](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[FloWorks 入门](getting-started.md){ .md-button }
[主界面解剖](interface-anatomy.md){ .md-button .md-button--primary }
[理念](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## 什么是 FloWorks？
FloWorks 是一个**开源可视化实验室**（Python + PySide6），在这里您可以通过连接模块（节点）来构建系统，而无需编写代码行。

想象一个数字画布，您可以在其中通过虚拟线缆将信号发生器、数学滤波器、硬件控制器（VISA/SCPI）和人工智能模型连接起来。一切都基于**数据流**：将一个模块的输出连接到另一个模块的输入，以处理信息、自动化设备或实时分析结果。

它面向学生、研究人员、工程师以及任何希望以直观方式实验、学习或原型化复杂系统的人，无需传统编程的门槛。

### 使命
将实验工作流程集中在一个可视化、开放且易用的工具中。我们希望户专注于*实验和发现*，而不是与软件复杂性或许可成本作斗争。

### 愿景
一个世界，其中实验想法与执行之间的唯一障碍是实验者的好奇心。FloWorks 立志成为科学和工程领域的参考平台，由全球社区构建、服务于全球社区，打破专有工具的壁垒。

### 原则
* **完全自由（MIT 许可证）：** 知识和工具必须对所有人免费且可访问。
* **无限可扩展性：** 如果缺少某个模块，任何人都可以使用 Python 创它并将其集成到生态系统中。
* **视觉透明性：** 过程的每一步都可以以图形方式进行检视、调试和理解。
* **与现实世界的连接：** 这不仅仅是仿真；它还允许直接从画布控制真实的科学仪器。

与封闭或高度专业化的工具不同，FloWorks 被设计为一个可扩展的模块化生态系统，其中每个组件都是一个可重用、可连接的节点。

---

## 主要功能

<div class="grid cards" markdown>

-   **:material-puzzle-outline: 可扩展节点生态系统**

    按层组织的技术目录：源处理、控制、硬件和脚本。

    动态注册、声明式序列化和清晰的契约，用于快速开发。

    [:material-arrow-right: 节点参考](node-reference.md)

-   **:material-connection: VISA/SCPI 集成**

    与示波器、LCR 表和发生器直接连接。

    多通道支持，通过 `PyVISA-py` 进行集成仿真，以及便携模式下的防火墙管理。

    [:material-arrow-right: 仪器](instrumentation.md)

-   **:material-package-variant-closed: 便携式 `.sflow` 格式**

    包含 JSON 图、`.npy` 数组和元数据的自包含 ZIP 标准。

    验的完全可重复性和自动 DPI 标准化。

    [:material-arrow-right: .sflow 格式](sflow-format.md)

-   **:material-translate: 高级国际化**

    无需重启应用即可进行热语言切换。

    分层 JSON 翻译和偏好持久化。

    [:material-arrow-right: i18n 指南](translation-guide.md)

-   **:material-tools: SDK 和快速开发**

    基础模板（`template_node.py`）、序列化混入和分步指南。

    为插件和社区扩展准备的架构。

    [:material-arrow-right: 创建节点](adding-a-new-node.md)

</div>

---

## 应用领域

| 领域 | 应用 |
|------|------|
| 🎓 **教育** | 物理、电子、数学、STEM 实验室 |
| ⚙️ **工程** | 数字信号处理、控制、仪器、计量 |
| 🤖 **人工智能** | 机器学习、优化、混合流水线 |
| 🔬 **研究** | 自动化和数据采集 |
| 🔌 **硬件** | VISA/SCPI、仿真和混合系统 |

---

!!! tip "FloWorks 新手？"

    从 **FloWorks 入门** 部分开始，然后阅读 **界面解剖** 以了解图形界面架构，最后探索 **总体架构** 以理解数据流和拓扑引擎结构。

---

!!! info "Open Core 模式"

    FloWorks 采用 **Free/Open Core** 模式，基于 **MIT 许可证**。

    核心保持免费和开放，而未来的企业、课程或市场扩展将是可选的。

---

<div markdown="1" style="text-align: center;">

## FloWorks

可视化处理 • 仪器 • 科学 • 人工智能

<small>使用 MkDocs Material 构建的文档</small>

</div>
