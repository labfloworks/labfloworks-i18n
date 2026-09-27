---
title: .sflow 文件格式
description: FloWorks 交换标准的技术规范、内部结构和使用指南
---

# 📄 `.sflow` 文件格式

`.sflow` 格式是 **FloWorks** 的原生交换和持久化标准。它允许将完整的工作流打包到单个文件中，包括图拓扑、节点参数、处理后的数据和便利贴，从而便于共享、存档或确定性地复现实验。

---

## 📦 什么是 `.sflow` 文件？

`.sflow` 文件本质上是一个**重命名的 ZIP 文件**。将其扩展名改为 `.zip` 后，您可以使用任何文件管理器或命令行工具检查其内容。

其最小内部结构包括：

| 组件 | 描述 |
|-----------|-------------|
| `diagram.json` | 主清单：定义节点、连接、视图、便利贴和序列化元数据。 |
| `data/` | 包含每个节点数据的文件夹，格式为 `.npy`（NumPy 二进制数组）。 |
| `metadata.json` *(可选)* | 补充信息：作者、FloWorks 版本、描述和标签。 |

=== "🌳 视觉结构"
    ```text
    my-flow.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (可选，用于 ScriptNode 持久化)
    ```

---

## 🧩 `diagram.json` – 流的核心

此 JSON 文件描述了完整的拓扑结构、画布上元素的位置以及保存时的图状态。

### 最小示例
```json
{
  "nodes": [
    {
      "id": "n1",
      "type": "OscilloscopeNode",
      "pos": [150, 200],
      "params": { "channel": "primary", "simulation": false }
    },
    {
      "id": "n2",
      "type": "GraphExporterNode",
      "pos": [450, 200],
      "params": { "theme": "dark", "export_format": "png" }
    }
  ],
  "connections": [
    {
      "from": "n1",
      "to": "n2",
      "from_port": "out",
      "to_port": "data_in"
    }
  ],
  "viewport": { "x": 0, "y": 0, "scale": 1.0 },
  "stickers": [
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "检查阈值", "user_modified": true }
  ]
}
```

### 主要字段
| 字段 | 类型 | 描述 |
|-------|------|-------------|
| `nodes` | `Array` | `{id, type, pos, params}` 对象列表。`type` 必须与 `node_registry.py` 匹配。 |
| `connections` | `Array` | `{from, to, from_port, to_port}` 连接列表。端口是字符串，不是索引。 |
| `viewport` | `Object` | `(x, y, scale)`，用于恢复画布的确切位置和缩放。 |
| `stickers` | `Array` | 序列化的便利贴，坐标归一化为 96 dpi。 |

!!! tip "节点序列化"
    每个节点的特定参数通过 `SerializableMixin` 管理。仅保存 `SERIALISABLE = [...]` 中声明的属性。系统中未注册的节点在加载期间会自动跳过。

---

## 💾 `data/` – 处理后的数据和 NumPy 数组

执行流时，节点可以将其结果存储在此文件夹中的 `.npy` 文件内。

- 文件名通常与节点的 `id` 或内部引用匹配。
- 数组以 NumPy 二进制格式存储，**严格保留原始维度**（1D、2D、3D 等）。引擎永远不会应用 `flatten()`。
- 在 `diagram.json` 中，数据通过 `__npy__:` 前缀引用：
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- 如果节点不产生数据或配置为不持久化，则可以省略相应的文件。

??? note "外部兼容性"
    `.npy` 文件 Python 生态系统中是通用的。您可以在 FloWorks 外部使用以下方式读取它们：
    ```python
    import numpy as np
    data = np.load("data/node_1.npy")
    print(data.shape)
    ```

---

## 🏷️ `metadata.json`（可选）

包含不影响执行的描述性信息，非常适合可追溯性和项目管理：

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "三相电机振动分析",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["engineering", "vibrations", "FFT", "multichannel"]
}
```

---

## 🔄 保存和加载过程

FloWorks 实现了健壮的机制来保证数据完整性：

1. **保存：**
   - 遍历图并通过 `SerializableMixin` 序列化节点。
   - 将数组提取到 `data/` 并通过 `__npy__:` 在 JSON 中引用。
   - 将所有内容打包为扩展名为 `.sflow` 的 ZIP 文件。
2. **安全加载：**
   - 创建当前图的**临时内存备份**。
   - 提取并解析新的 `.sflow`。
   - 如果发生任何错误（JSON 无效、节点缺失、`.npy` 损坏），**会自动恢复备份**，不会丢失工作。
3. **特殊的 ScriptNode：**
   - 保存 `script`、`params`、`dynamic_inputs`、`dynamic_outputs`、`persist` 和 `python_path`。
   - 加载时，它会重新编译代码、重建动态端口并自动恢复 `persist` 状态。
4. **StickyNotes 和 DPI：**
   - 保存时，坐标和大小归一化为 **96 dpi**。
   - 加载时，它们会缩放到当前显示器的 DPI，从而保证不同分辨率之间的视觉一致性。

---

## 🛠️ 外部使用和自动化

`.sflow` 格式设计为透明且可编程。您可以从外部脚本读取或生成它：

=== "🐍 Python（读取）"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("my-flow.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        data_n1 = np.load(z.open("data/node_1.npy"))
        print(f"节点数: {len(graph['nodes'])}")
        print(f"数据: {data_n1.shape}")
    ```

=== "📤 Python（基本创建）"
    ```python
    import zipfile
    import json
    import numpy as np

    graph = {
        "nodes": [{"id": "gen", "type": "GeneratorNode", "pos": [100, 100], "params": {}}],
        "connections": [],
        "viewport": {"x": 0, "y": 0, "scale": 1.0}
    }

    with zipfile.ZipFile("new.sflow", "w", zipfile.ZIP_DEFLATED) as z:
        z.writestr("diagram.json", json.dumps(graph, indent=2))
        z.writestr("data/gen.npy", np.array([1.0, 2.0, 3.0]))
    ```

---

## 🔮 兼容性和未来可扩展性

`.sflow` 格式遵循**可扩展和向后兼容的设计**原则：

- ✅ **新章节：** 未来版本可以添加 `thumbnails/`、`logs/` 或 `plugins/` 等文件夹，而不会破坏旧的加载器。
- ✅ **可选字段：** 解析器忽略 `diagram.json` 中的未知键，允许添加实验性元数据。
- ✅ **版本控制：** `metadata.json` 中的 `floworks_version` 字段允许应用程序在格式演进时应用自动迁移。

!!! warning "黄金法则"
    应用程序打开时，切勿手动修改 `diagram.json`。系统依赖于拓扑、数组和视图状态之间的一致性。请始终使用原生的保存/加载流程。

---

## 📚 相关资源
- [🗺️ 代码地图和架构](architecture-ii.md) → `file_io.py` 和 `SerializableMixin` 如何管理格式。
- [📦 可移植构建指南](guia-ejecutable-portable.md) → 资源打包和安全路径。
- [🧩 节点参考](node-reference.md) → 种节点类型的序列化约定。
