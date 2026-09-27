---
title: VISA/SCPI 仪器
description: 通过 VISA/SCPI 标准在 FloWorks 中连接、配置和使用真实和模拟硬件的指南。
---

# 🔌 VISA/SCPI 仪器

FloWorks 通过 **VISA**（虚拟仪器软件架构）抽象层上的 **SCPI**（可编程仪器的标准命令）协议，集成了与真实实验室仪器的直接通信。它还提供纯基于 Python 的仿真器，用于开发、测试和共享流程，而无需物理硬件。

---

## 🌐 什么是 VISA/SCPI？

| 技术 | 描述 |
|------------|-------------|
| **VISA** | 抽象物理接口（USB-TMC、以太网/LAN、GPIB、RS‑232）的标准层。只需修改连接字符串即可从真实仪器切换到模拟仪器。 |
| **SCPI** | 用于控制发生器、示波器、万用表、LCR 表等的标准化 ASCII 命令语言。制造商扩展了该标准，但基础是通用的。 |
| **PyVISA** | FloWorks 使用的 Python 后端。支持 `@py`（纯仿真）和原生后端（`@ni`、`@ivi`、`@keysight` 等）。 |

---

## ⚙️ 典型配置

=== "📍 连接字符串 (Resource String)"
    标准 VISA 式：
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR`（USB 示波器）
    - `TCPIP0::192.168.1.100::inst0::INSTR`（局域网/以太网）
    - `ASRL1::INSTR`（RS-232 串口）
    - `GPIB0::1::INSTR`（传统 GPIB）

=== "⏱️ 超时和选项**"
    - **超时**：以毫秒为单位可配置。如果仪器需要长时间测量或频率扫描，请增加此值。
    - **初始化**：某些节点允许在连接时注入自定义 SCPI 命令（例如 `*CLS`、`SYST:PRES`、`:CHAN1:DISP ON`）。

---

## 📡 可用硬件节点

<div class="grid cards" markdown>

- **🔭 SCPI 示波器**
  捕获时域波形。支持多通道、自动缩放、硬件触发和"显示通道"菜单，可即时切换信号。

- **⚡ LCR 表**
  测量阻抗、电感、电容、电阻和损耗因子。在单次采集中返回包含主要和辅助数据的 `master_payload`。

- **🎛️ 任意函数发生器**
  向 SDG 硬件发送信号或模拟输出。配置调制（AM/FM/PM）、线性/对数扫描、突发和相位。

- **📊 数字万用表 (DMM)** *(扩展中)*
  用于 DC/AC 电压电流、电阻和频率测量的 SCPI 接口。兼容 Keithley、Agilent 和 Rigol。

- **🔋 可编程电源** *(扩展中)*
  具有 OVP/OCP 保护的输出电压/电流控制。适用于自动化测试台。

</div>

---

## 🔄 典型工作流

1. **添加节点** 到画布，从工具栏（`源` 或 `仪器`）。
2. **配置连接**：选择后端，输入 VISA 字符串并调整超时/初始化。
3. **连接到流**：将仪器输出链接到处理节点（FFT、滤波器、算术）或可化节点。
4. **运行 (`F5`)**：拓扑引擎请求采集，驱动程序解析 SCPI 响应并打包数据。
5. **可视化/导出**：数据流经图，由后续节点处理。

---

## 🛠️ 故障排除

!!! warning "1. VISA 找不到仪器 (`VI_ERROR_RSRC_NFOUND`)"
    - **原因：** 字符串不正确、电缆断开或后端未检测到设备。
    - **解决方案：** 运行 `pyvisa-shell` 或制造商的工具（NI MAX、Keysight Connection Expert）以列出有效资源。验证用户权限。

!!! warning "2. 采集期间超时"
    - **原因：** 扫描缓慢、未满足触发条件或仪器正忙于其他任务。
    - **解决方案：** 在节点中增加超时。验证示波器触发是否配置正确（`AUTO` 或 `NORMAL`）。启动时使用 `*CLS`。

!!! warning "3. 模拟无响应或失败"
    - **原因：** `PyVISA-py` 未安装或与其他后端存在冲突。
    - **解决方案：** `pip install pyvisa-py`。在节点中，显式选择 `@py` 作为后端。

!!! warning "4. SCPI 错误 (`Command Error`, `Execution Error`)"
    - **原因：** 固件不支持命令或语法不正确。
    - **解决方案：** 查阅仪器的 SCPI 编程手册。某些制造商需要 `:` 前缀或 `
` 终止符。FloWorks 会自动添加 `
`，但您可以在驱动程序中调整终止符。

!!! info "5. 为不支持的仪器创建节点"
    - 继承自 `BaseNode` 并在 `instrument/` 中使用 `DeviceBase` 模式。
    - 实现一个返回 `(x, y)` 元组或 `master_payload` 的 `headless` 驱动程序。
    - 按照 [📘 指南：添加新节点](adding-a-new-node.md) 注册端口、序列化和 i18n。

---

## 📚 相关资源

- [🧩 技术节点参考](node-reference.md) → `oscilloscope_node`、`generator_node` 和序列化约定的详细信息。
- [📦 可移植构建指南](guia-ejecutable-portable.md) → 防火墙处理、`resource_path()` 和 PyInstaller 打包。
- [📘 添加新节点](adding-a-new-node.md) → 如何扩展 `instrument/` 并注册自定义驱动程序。
- [📄 `.sflow` 格式](sflow-format.md) → 硬件配置和捕获数组的持久化方式。
