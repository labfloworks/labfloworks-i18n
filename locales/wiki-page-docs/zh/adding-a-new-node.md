---
title: 向 FloWorks 添加新节点指南
description: 在 FloWorks 流引擎中创建、注册和集成自定义节点的分步教程。
---

# 📘 开发者指南：如何向 FloWorks 添加新节点

本指南描述了在 FloWorks 中创建新节点类型的完整流程，确保其与流引擎、用户界面、视觉主题和国际化系统正确集成。

---

## 📋 目录
- [📘 开发者指南：如何向 FloWorks 添加新节点](#-开发者指南如何向-floworks-添加新节点)
  - [📋 目录](#-目录)
  - [1. 架构简介](#1-架构简介)
  - [2. 使用 `template_node.py` 模板](#2-使用-template_nodepy-模板)
  - [3. 分步创建自定义节点](#3-分步创建自定义节点)
    - [3.1. 复制并重命名模板](#31-复制并重命名模板)
    - [3.2. 定义端口和标签](#32-定义端口和标签)
    - [3.3. 实现处理逻辑](#33-实现处理逻辑)
    - [3.4. 自定义外观（可选）](#34-自定义外观可选)
    - [3.5. 添加可配置参数（可选）](#35-添加可置参数可选)
    - [3.6. 使节点可序列化（保存 / 加载配置）](#36-使节点可序列化保存--加载配置)
  - [4. 系统集成](#4-系统集成)
  - [5. 国际化（i18n）](#5-国际化i18n)
  - [6. 视觉主题](#6-视觉主题)
  - [7. 检查清单和故障排除](#7-检查清单和故障排除)
    - [✅ 检查清单](#-检查清单)
    - [🐛 常见问题](#-常见问题)
  - [8. 结论](#8-结论)

---

## 1. 架构简介

FloWorks 基于 PySide6 构建，使用可连接节点模型来表示信号处理流。

---

## 2. 使用 `template_node.py` 模板

为便于创建新节点，提供了文件 `nodes/template_node.py`。该模板包括：
- 完整的国际化支持（连接到 `languageChanged`，`update_language` 方法）。
- 完整的主题支持（`update_theme` 方法）。
- 内置帮助，采用三段式 HTML 格式。
- 管理多个可配置的输入/输出端口。
- 通过 `get_output_for_port` 实现多输出。
- 通过 `get_display_signal` 在绘图中进行可视化。
- 可翻译的上下文菜单。

建议在开发新节点时始终从此模板开始。

---

## 3. 分步创建自定义节点

### 3.1. 复制并重命名模板
1. 将 `nodes/template_node.py` 复制为新节点的名称，例如 `nodes/my_node.py`。
2. 将类名从 `TemplateNode` 重命名为描述性名称，例如 `MyNodeNode`。
3. 如有必要，调整导入。

### 3.2. 定义端口和标签
!!! warning "重要：名称匹配"
    `PORTS`、`PORT_LABELS` 中的端口名称以及 `execute_program` 返回的字典键必须**完全相同**（包括大小写）。模板现在包含别名映射（`'data_in'` → 第一个左侧端口）以提高健壮性。

编辑文件顶部的 `PORTS` 字典。每个条目的格式为：
```python
"port_name": ("side", fraction)
```
- **可能的边：** `"left"`、`"right"`、`"top"`、`"bottom"`。
- **fraction：** 介于 `0.0` 和 `1.0` 之间的值，表示沿边的位置。

**一个输入和两个输出的节点示例：**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
`PORT_LABELS` 字典包含将示在每个端口旁边的文本。建议使用翻译键而不是固定文本（参见国际化部分）。

### 3.3. 实现处理逻辑
关键方法是 `execute_program(self, input_data)`。当节点接收数据时，流引擎会调用此方法。

**`input_data` 可以是：**
- 如果没有输入，则为 `None`。
- 对于时间信号，为 `(x, y)` 元组。
- 一维数组。
- 对于多输入节点，为 `{port_name: data}` 字典。

**返回值：**
- 对于单输出节点，直接返回数据（例如组 `(x, y)`）。
- 对于多输出节点，返回一个字典，其中键与 `PORTS` 中定义的输出端口名称匹配。

```python
def execute_program(self, input_data):
    # 处理 input_data 并生成结果
    magnitude_result = (freq, mag)
    phase_result = (freq, phase)
    return {
        "magnitude": magnitude_result,
        "phase": phase_result
    }
```

!!! tip "关于通用端口名称的说明"
    流引擎有时会传递一个带有诸如 `'data_in'` 之类的键的字典，而不是真实的端口名称（特别是如果用户没有精确点击圆圈）。模板已包含处理此情况的代码：
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    这可防止节点因连接不精确而失败。

模板已包含一个注释示例。它还实现了 `get_output_for_port(self, port_name)`，以便引擎可以路由每个输出：
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. 自定义外观（可选）
`paint()` 方法绘制背景、标题、状态和任何附加文本。您可以修改：
- 颜色（通过 `update_theme` 自动更新）。
- 状态文本（使用 `self._status` 属性）。
- 摘要信息（例如幅度峰值）。

模板显示了一个基本示例。

### 3.5. 添加可配置参数（可选）
如果您的节点需要用户可调整的参数（例如窗口大小、截止频率），您可以：
1. 在 `__init__` 中添加属性（例如 `self.window_size = 512`）。
2. 创建配置对话框（继承自 `QDialog`）。
3. 在 `open_config_dialog()` 中连接对话框（模板中已存在的方法）。
4. 从对话框更新参数并调用 `self.update()`。

### 3.6. 使节点可序列化（保存 / 加载配置）
为了使节点能够在复制/粘贴、撤消/重做或使用文件菜单中的保存/打开命令时保存和恢复其参数，它必须继承自序列化混入并声明其属性。

1. 在文件中导入混入：
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. 更改类继承以在 `QGraphicsObject` 之前包含它：
    ```python
    class MyNodeNode(SerializableMixin, QGraphicsObject):
    ```
3. 在类级别定义 `SERIALISABLE` 列表，其中包含您要持久化的属性名称。仅支持简单类型（`int`、`float`、`str`、`bool`）、列表、字典或 NumPy 数组（后者自动存储为 `.sflow` 内的 `.npy` 文件）。
    ```python
    class MyNodeNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frequency', 'amplitude', 'configuration']
    ```
4. 确保这些属性在 `__init__` 中初始化：
    ```python
    self.frequency = 1000.0
    self.amplitude = 1.0
    self.configuration = {'type': 'sine', 'phase': 0}
    ```

这样，您无需编写 `serialize`/`deserialize` 方法；混入会自动处理保存和恢复值。

如果您的节点在加载时需要额外的逻辑（例如，重新连接硬件仪器），您可以通过首先调用父方法来覆盖 `deserialize`：
```python
def deserialize(self, data):
    super().deserialize(data)   # 恢复 SERIALISABLE 属性
    self._start_device()
```

---

## 4. 系统集成

创建节点文件后，您只需将其放入 `nodes` 文件夹中，它就会出现在界面中并与系的其余部分一起工作。

---

## 5. 国际化（i18n）

所有可见文本必须通过 `tr("key", default="...")` 进行翻译。模板已实现此功能。您必须在 `locales/` 内的 JSON 文件中添加相应的键。

**推荐结构：**
```json
{
   "nodes": {
     "my_node": {
       "title": "My Node",
       "tooltip": "Tooltip description",
       "ports": {
         "input": "Input",
         "output1": "Output 1",
         "output2": "Output 2"
      },
       "status": {
         "no_data": "No data",
         "ready": "Ready"
      },
       "menu": {
         "show_output": "Show output",
         "configure": "Configure..."
      },
       "help_title": "Help - My Node",
       "help_html": "<h3>🎛️ Filter Node</h3>
<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_my_node": "My Node"
  }
}
```

HTML 帮助遵循所有节点通用的三段式格式（特定描述 + "如何思考系统" + "快捷方式和技巧"）。模板已在 `get_help_text()` 中包含该结构。

---

## 6. 视觉主题

`update_theme(self, theme)` 方法接收一个字典，其中包含当前主题定义的颜色。模板会自动更新：
- 节点背景（`node_normal_bg`）
- 边框（`node_selected_border`）
- 标题和文本颜色（`node_normal_text`）
- 端口颜色（`port_circle`、`port_outline`、`port_inline`、`port_text`）

确保在 `MainWindow`（或 `ThemeUpdater`）中，主题更改时为每个节点调用 `node.update_theme()`。

---

## 7. 检查清单和故障排除

### ✅ 检查清单
- [ ] 节点可以从工具栏正确创建。
- [ ] 端口显示在预期位置，并且可以检测到以进行连接（`Ctrl+单击`）。
- [ ] 接收输入数据时，会调用 `execute_program` 并处理信号。
- [ ] 输出正确传播到连接的节点。
- [ ] 上下文菜单允许更改显示通道（如果有多个输出）。
- [ ] 单击节点时，所选信号会绘制在绘图小部件中。
- [ ] 双击可打开格式正确的帮助。
- [ ] 语言正确更改（标题文本、端口、菜单）。
- [ ] 主题正确更改（节点和端口颜色）。
- [ ] 复制/粘贴无错误地工作。

!!! tip "精确的端口连接"
    连接节点时，确保精确单击目标端口圆圈。如果单击节点主体，系统将使用通用名称（`'data_in'`）。模板现在可以容忍这些名称，但直接连接到圆圈是一种良好实践，以确保多输出的正确路由。

### 🐛 常问题

| 症状 | 可能原因 | 解决方案 |
|---------|---------------|----------|
| 连接箭头未锚定到端口。 | 端口圆圈没有 `setData(0, port_name)` 或未实现 `get_port_scene_pos`。 | 验证在 `_create_ports` 中是否执行了 `circle.setData(0, port_name)`，并且 `get_port_scene_pos` 使用了该名称。 |
| 输出未到达连接的节点。 | `execute_program` 未返回字典（对于多输出）或未实现 `get_output_for_port`。 | 确保 `execute_program` 返回 `{port_name: data}`，并且 `get_output_for_port` 返回相应的值。 |
| 单击节点时未绘制任何内容。 | `get_display_signal` 未返回有效的 `(x, y)` 元组，或 `display_channel` 与现有输出不匹配。 | 检查 `get_display_signal` 是否使用所选通道，并且数据是 NumPy 数组。 |
| 更改语言时文本未更新。 | 未连接 `languageChanged` 信号，或 `update_language` 未更新元素。 | 验证 `__init__` 中的连接：`language_manager.languageChanged.connect(self.update_language)`。 |
| 主题未应用。 | 创建节点或更改主题时未调用 `update_theme`。 | 在 `MainWindow` 中，创建节点后，调用 `node.update_theme(self.theme_manager.current_theme())`。 |
| 箭头指向节点中心。 | 单击了主体而不是圆圈，或名称与 `PORTS` 不匹配。 | 直接单击圆圈。验证 `get_port_scene_pos` 是否具有别名映射。 |
| 导入时出现 `NameError: name 'self' is not defined`。 | 实例属性在 `__init__` 外部声明。 | 所有诸如 `self.my_parameter` 的属性必须在 `__init__` 内定义。 |
| 复制/打开 `.sflow` 时参数丢失。 | 节点未继承 `SerializableMixin` 或未定义 `SERIALISABLE`。 | 实现本指南的第 3.6 步。 |

---

## 8. 结论

通过遵循本指南并使用 `template_node.py` 模板，您将能够高效且与系统其余部分一致地向 FloWorks 添加新节点。始终记得保持与 i18n 和主题的兼容性，以提供专业用户体验。

欢迎贡献您自己的节点！
