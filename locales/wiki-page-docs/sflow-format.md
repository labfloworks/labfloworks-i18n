---
title: .sflow File Format
description: Technical specification, internal structure and usage guide for the FloWorks interchange standard
---

# 📄 `.sflow` File Format

The `.sflow` format is the native interchange and persistence standard of **FloWorks**. It allows packaging a complete workflow into a single file that includes the graph topology, node parameters, processed data and sticky notes, making it easy to share, archive or reproduce experiments deterministically.

---

## 📦 What is a `.sflow` file?

A `.sflow` file is, in essence, a **renamed ZIP file**. By changing its extension to `.zip`, you can inspect its contents with any file manager or command-line tool.

Its minimum internal structure consists of:

| Component | Description |
|-----------|-------------|
| `diagram.json` | Main manifest: defines nodes, connections, view, sticky notes and serialization metadata. |
| `data/` | Folder with each node's data in `.npy` format (NumPy binary arrays). |
| `metadata.json` *(optional)* | Complementary information: author, FloWorks version, description and tags. |

=== "🌳 Visual Structure"
    ```text
    my-flow.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (optional, for ScriptNode persist)
    ```

---

## 🧩 `diagram.json` – The Heart of the Flow

This JSON file describes the complete topology, the position of elements on the canvas and the view state at the time of saving.

### Minimal Example
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
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "Check threshold", "user_modified": true }
  ]
}
```

### Main Fields
| Field | Type | Description |
|-------|------|-------------|
| `nodes` | `Array` | List of objects `{id, type, pos, params}`. `type` must match `node_registry.py`. |
| `connections` | `Array` | List of connections `{from, to, from_port, to_port}`. Ports are strings, not indexes. |
| `viewport` | `Object` | `(x, y, scale)` to restore the exact canvas position and zoom. |
| `stickers` | `Array` | Serialized sticky notes with coordinates normalized to 96 dpi. |

!!! tip "Node Serialization"
    Each node's specific parameters are managed via `SerializableMixin`. Only attributes declared in `SERIALISABLE = [...]` are saved. Nodes not registered in the system are automatically skipped during loading.

---

## 💾 `data/` – Processed Data and NumPy Arrays

When a flow is executed, nodes may store their results in `.npy` files inside this folder.

- The file name usually matches the node's `id` or internal references.
- Arrays are stored in NumPy binary format, **strictly preserving the original dimensionality** (1D, 2D, 3D, etc.). The engine never applies `flatten()`.
- In `diagram.json`, data is referenced with the `__npy__:` prefix:
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- If a node does not produce data or is configured not to persist it, the corresponding file may be omitted.

??? note "External Compatibility"
    `.npy` files are universal in the Python ecosystem. You can read them outside FloWorks with:
    ```python
    import numpy as np
    data = np.load("data/node_1.npy")
    print(data.shape)
    ```

---

## 🏷️ `metadata.json` (Optional)

Contains descriptive information that does not affect execution, ideal for traceability and project management:

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "Vibration analysis in three-phase motor",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["engineering", "vibrations", "FFT", "multichannel"]
}
```

---

## 🔄 Save and Load Process

FloWorks implements a robust mechanism to guarantee data integrity:

1. **Saving:**
   - The graph is traversed and nodes are serialized via `SerializableMixin`.
   - Arrays are extracted to `data/` and referenced in JSON with `__npy__:`.
   - Everything is packaged into a ZIP with `.sflow` extension.
2. **Safe Loading:**
   - A **temporary in-memory backup** of the current diagram is created.
   - The new `.sflow` is extracted and parsed.
   - If any error occurs (invalid JSON, missing nodes, corrupted `.npy`), **the backup is automatically restored** with no loss of work.
3. **Special ScriptNode:**
   - Saves `script`, `params`, `dynamic_inputs`, `dynamic_outputs`, `persist` and `python_path`.
   - On load, it recompiles the code, rebuilds dynamic ports and restores the `persist` state automatically.
4. **StickyNotes & DPI:**
   - Coordinates and sizes are normalized to **96 dpi** on save.
   - On load, they are scaled to the current monitor's DPI, guaranteeing visual consistency across different resolutions.

---

## 🛠️ External Use and Automation

The `.sflow` format is designed to be transparent and programmatic. You can read or generate it from external scripts:

=== "🐍 Python (Reading)"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("my-flow.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        data_n1 = np.load(z.open("data/node_1.npy"))
        print(f"Nodes: {len(graph['nodes'])}")
        print(f"Data: {data_n1.shape}")
    ```

=== "📤 Python (Basic Creation)"
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

## 🔮 Compatibility and Future Extensibility

The `.sflow` format follows the principles of **extensible and backward-compatible design**:

- ✅ **New sections:** Future versions may add folders like `thumbnails/`, `logs/` or `plugins/` without breaking older loaders.
- ✅ **Optional fields:** The parser ignores unknown keys in `diagram.json`, allowing experimental metadata to be added.
- ✅ **Versioning:** The `floworks_version` field in `metadata.json` lets the application apply automatic migrations if the format evolves.

!!! warning "Golden Rule"
    Never manually modify `diagram.json` while the application is open. The system depends on consistency between topology, arrays and view state. Always use the native save/load flows.

---

## 📚 Related Resources
- [🗺️ Code Map and Architecture](architecture-ii.md) → How `file_io.py` and `SerializableMixin` manage the format.
- [📦 Portable Build Guide](guia-ejecutable-portable.md) → Packaging and safe paths for resources.
- [🧩 Node Reference](node-reference.md) → Serialization contracts per node type.
