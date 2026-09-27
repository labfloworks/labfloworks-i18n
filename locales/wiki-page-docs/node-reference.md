---
title: Technical Node Reference
description: Updated catalog, extension contracts and advanced capabilities of the FloWorks node system
---

# 🧩 Technical Node Reference

FloWorks does not depend on a static catalog. It uses a **dynamic registration system** based on clear contracts. This allows extending the platform without touching the topological engine. Below is the implemented catalog, real technical capabilities and the protocol for extending it safely.

---

## 📂 Core Categories

=== "📦 Layer View"
    <div class="grid cards" markdown>

    - **📥 Sources/Input**
      Generate or capture initial signals. Support integrated simulation, real hardware (VISA/SCPI) and multichannel mode.
    - **⚙️ Processing**
      Transform, combine or analyze data. Preserve dimensionality and automatically interpolate when necessary.
    - **🔀 Control/Flow**
      Branch, iterate or condition execution. Include native support for activation signals.
    - **🐍 Scripting/Advanced**
      Execute dynamic Python code with parametric ports (`# @param`), dynamic ports (`# @input`/`# @output`) and state persistence (`persist`).
    - **🔌 Hardware/Instrumentation**
      Interfaces for oscilloscopes, LCR meters and generators.
    - **📤 Output/Export**
      Visualize, export or archive results. Support visual themes, user profiles and professional format (PNG/PDF/SVG).

    </div>

---

## 📋 Implemented Technical Catalog

| Node | Type | Main Responsibility | Key Features |
|------|------|---------------------------|------------------------|
| `SumNode` | Processing | Arithmetic operator (+, -, *, /) for two inputs. | Automatically interpolates signals of different resolution (FFTs). Preserves dimensionality. |
| `RhombusNode` | Control | Conditional (Yes/No branch). | Two output ports. Evaluates condition by threshold or boolean logic. |
| `TriggerNode` | Control | Iterator/accumulator with external activation. | Receives `(x, y, "trigger")`. Accumulates up to N iterations and emits stacked/averaged result. |
| `ScriptNode` | Advanced | Integrated Python scripting environment. | QScintilla, autocompletion, `# @param`, dynamic ports, `persist`, templates, error console, external interpreter with timeout. |
| `OscilloscopeNode` | Hardware | Capture from oscilloscopes (SDS) or LCR meters. | Simulation mode, integrated firewall dialog, **multichannel support** (`out_primary`, `out_secondary`), "Show channel" menu. |
| `GeneratorNode` | Source | Sends signals to generators (SDG) or simulates outputs. | Modulation/sweep configuration, integrated simulation dialog. |
| `GraphExporterNode` | Output | Professional graph exporter. | Double-click configuration, custom axes, themes, saved profiles, etc. |

---

## 🔍 ScriptNode: Essential Capabilities

> **🐍 Integrated Scripting Environment**
>
> - **Built-in code editor:** Basic syntax highlighting, line numbering and code folding.
> - **Dynamic parameters panel:** Directives `# @param NAME : type = value` inject editable controls (spinbox, text field, etc.) into the side panel.
> - **Dynamic ports:** `# @input name` and `# @output name` create ports in real time. The script receives an `inputs` dictionary and returns `outputs`.
> - **State persistence:** Global `persist` dictionary that keeps values between runs.
> - **Templates and Import/Export:** Dropdown menu with base scripts. The user can save their scripts in `nodes/script_node/templates/` or import/export external `.py` files.
> - **Built-in error console:** Shows syntax/execution failures with the exact line highlighted in the editor.
> - **Help and i18n:** Contextual tooltips, `?` button with quick guide, and all texts use `tr()` for translation.
> - **External interpreter with timeout:** Configurable path (`# @python_path` or "Browse…" button). Isolated execution with time limit and fallback to internal interpreter.
> - **Complete serialization:** Saves script, parameters, dynamic ports and `persist` state. When loading a `.sflow`, it automatically rebuilds ports and parameters.

---

## 📚 Related Resources

- [📖 Code Map and Architecture](architecture-ii.md) → Responsibilities per module and workflows.
- [🌐 Internationalization (i18n) Guide](i18n.md) → How to add languages and manage `tr()` keys.
- [🛠️ Add a New Node (Tutorial)](adding-a-new-node.md) → Step by step with practical examples.
- [📦 Build and Distribution Guide](build.md) → PyInstaller packaging, hooks and digital signatures.

---

💡 **Is a node missing from this catalog?**
FloWorks is designed to be extensible. If you need a node that does not exist, create it following the `BaseNode` contract and register it. The community and the future marketplace will continuously expand the ecosystem without breaking compatibility.
