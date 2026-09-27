# 🧪 FloWorks 控制台交互式教程

欢迎来到 FloWorks 实验实验室。本节面向更高级的用户；它是一个 **Python** 终端，用于控制与画布相关的一切，即节点及其连接，以逐行、顺序的方式。它是一个与程序相连的终端，可以控制程序并确定行为或例程，以满足要求更高的用户。

本指南将逐步向您展示如何在不触碰鼠标的情况下控制和分析您的流程图。每个示例都已在交互式控制台中验证，并反映了程序的真实数据结构。

---

## 1. 了解环境

控制台注入了三个全局对象：`app`（主窗口）、`graph`（场景/图表）和 `selected_node`（当前在画布上选中的节点）。所有命令都基于这三个对象。

### 查看所有节点

```python
>>> graph.nodes
```

**示例输出：**
```
场景中的节点：
  [0] 高级信号发生器 (类型: SignalSourceNode, 类别: Sources)
  [1] FFT (类型: FFTNode, 类别: Processing)
```

方括号中的索引（`[0]`、`[1]`）是访问节点的主要方式。顺序是画布上的创建顺序。

#### 替代方法：统计节点或按类型过滤

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### 查看所有连接

```python
>>> graph.connections
```

**示例输出：**
```
场景中的连接：
  [0] 高级信号发生器 (out) → FFT (input)
```

输出显示源节点名称、输出端口、箭头、目标节点和输入端口。如果连接未出现，则流无法执行。

#### 替代方法：查看单个节点的连接

```python
>>> selected_node.connectors
```

### 查看选中的节点

单击画布上的一个节点，然后运行：

```python
>>> selected_node
```

**示例输出：**
```
节点: FFT
  类型: FFTNode
  类别: Processing
  端口: ['input', 'output', 'magnitude', 'phase']
```

> **💡 注意：** 如果未选择任何节点，`selected_node` 为 `None`。选择节点也会自动更新侧边参数表。

#### 替代方法：通过代码选择节点

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. 不使用鼠标操作节点和连接

### 创建新节点

您必须知道节点的确切类名（与目录中相同）。参数为：`(type, x, y)`。

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

节点出现在画布上的坐标 (300, 200) 处。如果您不知道确切名称，请列出类别（参见第 7 节）。

#### 替代方法：一次创建多个节点

```python
>>> for i, tipo in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(tipo, 100 + i*200, 300)
```

### 手动连接节点

语法：`graph.connect_nodes(source, destination, 'output_port', 'input_port')`。端口取决于每个节点；切勿假设其名称。

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 注意：** 连接前务必检查 `graph.nodes[N].PORTS`。FFT 节点有 `'input'` 和 `'magnitude'`；发生器有 `'output'`。

#### 替代方法：连接到默认端口

如果您不知道确切的输入端口名称，某些节点接受 `None` 以使用第一个可用端口：

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### 删除节点

```python
>>> graph.remove_node(graph.nodes[2])
```

删除节点及其所有关联连接。`graph.nodes` 的索引将重新排序，因此请勿保留旧引用。

#### 替代方法：删除某个类别的所有节点

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### 查看节点的端口

```python
>>> graph.nodes[1].PORTS
```

**示例输出：**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

键是端口名称（字符串）。值是具有视觉位置的元组。连接时只需关注键。

#### 替代方法：以简单列表形式查看端口

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. 执行流并查看结果

### 执行整个图

```python
>>> graph.execute_flow()
```

此方法属于图表（`graph`），而非主窗口。它按拓扑顺序遍历所有节点，执行每个节点并缓存结果。它不返回任何内容；数据在内部存储。

#### 替方法：强制计算特定分支

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

这会重新计算指定节点的整个上游树，并直接返回结果，而不修改全局缓存。

### 查看节点的缓存数据

如果您需要访问特定节点的处理数据，可以通过两种方式：

#### 直接方式（按对象）
```python
>>> graph.node_values[graph.nodes[1]]
```

**示例输出：**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ 警告：** 如果场景中节点的顺序发生变化（例如，通过删除或添加节点），或者对象实例与字典中存储的键不完全匹配，则此方法可能会因 `KeyError` 而失败。

#### 替代方式（按位置）
```python
>>> list(graph.node_values.values())[1]
```

这种方式**更稳定**，因为它不依赖于对象的确切身份。值顺序遵循上次 `graph.execute_flow()` 期间节点执行的顺序。索引 `[1]` 对应该序列中的第二个节点。

> **💡 注意：** 如果您想查看每个节点在执行顺序中的索引，可以使用：
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ 注意：** `graph.node_values` 并不总是直接返回数组。对于处理器节点（FFT、滤波器等），它返回一个**字典**，其中每个键都是一个输出端口。对于源节点，它返回一个元组 `(x, y)`。

#### 替代方法：在一行中查看所有节点的数据
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### 访问源节点的 Y 轴

发生器节点（SignalSourceNode、FileInputNode 等）在执行时返回一个元组 `(time, signal)`。要仅获取 Y 轴：

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**示例输出：**
```
Scalar NumPy (float64): 1.0
```

#### 替代方法：获取 X 轴（时间）

```python
>>> x = result[0]
>>> x[:5]
```

### Python 赋值：一个至关重要的细节

在 Python 中，赋值（`=`）是**语句**，不是表达式。控制台在 `x, y = ...` 之后不会打印任何内容，因为没有返回值。

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

要验证它是否有效，请在下一行评估变量：

```python
>>> x
>>> y.shape
```

或使用 `;` 在同一行中链接表达式：

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

或使用显式 `print()`：

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. 在主图中绘制

### 清除图表

```python
>>> app.plot_widget.clear_plot()
```

#### 替代方法：清除并立即重绘

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### 从控制台绘制任意信号

您可以使用 NumPy 创建数组，并将直接发送到绘图小部件，无需经过任何节点。

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### 替代方法：绘制正弦波之和

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### 绘制源节点的结果

由于源节点返回 `(x, y)`，您可以直接解包：

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### 绘制 FFT 节点的结果（多端口）

具有多个输出的节点（FFT、时频分析等）不会返回简单的元组。它们返回一个 `dict`，其中每个键都是一个输出端口。

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**示例输出：**
```
dict_keys(['output', 'magnitude', 'phase'])
```

请注意，如果节点没有通用端口，`'output'` 可能为 `None`。有用的输出是 `'magnitude'` 和 `'phase'`，它们反过来是元组 `(frequencies, values)`：

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### 替代方法：绘制相位而非幅度

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### 替代方法：叠加两个信号

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # 另一个节点
>>> app.plot_widget.plot_waveform(x2, y2)  # 叠加
```

> **💡 注意：** 如果您尝试对 dict 执行 `x, y = result`，Python 将引发 `ValueError: too many values to unpack`。解包前务必使用 `type(result)` 和 `result.keys()` 进行检查。

---

## 5. 即时修改应用程序

### 更新侧边参数表

如果您通过代码修改参数并希望侧边表反映更改：

```python
>>> app.workspace_table.populate()
```

此方法不接受任何参数。它使用选中节点的当前值刷新表格。

#### 替代方法：强制选择另一个节点并刷新

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### 从工具栏添加节点

```python
>>> app.add_node('SignalSourceNode')
```

这等效于按工具栏上的 "+" 按钮。节点放置在画布上的默认位置。

### 更改窗口标题

`setWindowTitle` 是 Qt 的原生方法。它可以工作，但请记住，应用程序可能有一个计时器或事件调用 `update_title()` 并自动覆盖它。

```python
>>> app.setWindowTitle('我的信号实验室')
>>> app.windowTitle()
```

要恢复应用程序根据其内部状态（项目名称、文件等）计算的"官方"标题：

```python
>>> app.update_title()
```

#### 替代方法：带项目名称的标题

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. 控制台中的导航和生产力

控制台不仅仅是一个 `print()`。它具有历史记录、自动完成和多行块功能。

| 按键 / 命令               | 操作                                                           |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | 浏览已执行命令的历史记录                                       |
| `Tab`                     | 自动完成命名空间中的变量、属性和方法                           |
| `Ctrl + L`                | 清除整个控制台（删除文本，不删除 Python 状态）                 |
| `if`, `for`, `def`, `class` | 提示符从 `>>>` 变为 `...`，用于多行块                         |
| `Ctrl+C`（在选中时）      | 复制控制台文本                                                 |
| `Ctrl+A`                  | 选择所有内容                                                   |

> **💡 注意：** 自动完成使用 `rlcompleter`，并识别整个注入的命名空间`app`、`graph`、`selected_node`）以及您在会话中定义的任何变量。

---

## 7. 高级技巧

### 更改节点的内部参数

节点参数不是平面属性。它们嵌套在 `params` 字典内，而该字典又有诸如 `'preset'`、`'formula'` 或 `'advanced'` 等子部分。切勿执行 `node.amplitude = 3.0`；这会在对象上创建一个新属性，但不会修改真实参数。

#### 情况 A：修改预设（正弦、方波等）

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### 情况 B：使用自定义公式

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

表达式使用 `t` 作为时间变量。`vars` 中的值是可以在公式中引用的符号。如果省略 `vars`，节点将使用默认值，公式可能无法反映更改。

#### 情况 C：更改高级参数（采样率、持续时间）

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### 情况 D：更改非发生器节点的参数（例如 FFT）

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 注意：** `getattr(obj, '_generate_signal', lambda: None)()` 是一种安全模式：如果方法存在（发生器节点），则调用它；否则，不执行任何操作，也不会引发错误。对于处理器节点，仅 `graph.execute_flow()` 就足够了。

### 列出所有可用的节点类别

导入会加载目录，但不会自动显示它。请记住，在 Python 中，成功的导入不会打印任何内容；您必须评估对象。

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

要查看可读的摘要：

```python
>>> for cat, nodes in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(nodes)} 节点")
```

#### 替代方法：按类别列出节点名称

```python
>>> {cat: [n.__name__ for n in nodes] for cat, nodes in NODE_CATEGORIES.items()}
```

### 查看任何方法的帮助

```python
>>> help(graph.connect_nodes)
```

文档字符串直接显示在控制台中。这对于发现方法期望的参数非常有用，无需打开源代码。

#### 替代方法：查看过滤后的属性

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. 如果出现问该怎么办？

- **控制台中的红色错误：** 显示完整的回溯。应用程序不会关闭；您可以更正命令并重试。
- **界面冻结：** 您可能写了一个无限循环。控制台在单独的线程中运行，但如果循环影响 GUI 线程，请重新启动应用程序。
- **意外的 `None`：** 如果节点返回 `None` 而不是数据，请验证它是否已在上游连接（`graph.connections`）以及流是否已执行（`graph.execute_flow()`）。
- **`ValueError: too many values to unpack`：** 您正试图将 dict 解包为元组。请先使用 `result.keys()`。
- **`ValueError: not enough values to unpack`：** 您期望 2 个值，但节点返回 1 个（dict）或 3 个（频谱图）。解包前请使用 `type(result)` 检查。
- **`AttributeError`：** 对象没有该属性。使用 `dir(obj)` 或 `[a for a in dir(obj) if 'word' in a.lower()]` 来发现正确的名称。
- **执行时没有任何反应：** 检查是否至少有一个源节点连接到链，并且已调用 `graph.execute_flow()`。处理器节点不会自行生成数据。
- **图表没有变化：** 确保在修改参数后调 `graph.execute_flow()`。仅更改 `params` 不会自动重新计算。

---

© 2026 FloWorks — 信号实验室
