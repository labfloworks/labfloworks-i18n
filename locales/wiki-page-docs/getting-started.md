---
title: Getting Started with FloWorks
description: Quick guide to set up the environment, run your first flow and access the portable version.
---

# 🚀 Getting Started with FloWorks

This guide will take you from zero to having your first signal processing flow running. FloWorks is a flow-diagram application for signals, built with Python and PySide6, that supports real hardware (VISA/SCPI), integrated simulation, advanced scripting and hot language switching.

---

## 🌊 Your first example flow

Let's create a simple flow: generate a sine wave signal and visualize it in real time.

1. **Add nodes**
   In the top toolbar, select `Source` → select `Advanced Signal Generator`. Then, from `Processing` → select, for example, `Spectral`.
2. **Connect**
   Do `Ctrl+Click` on the output port (`right`) of the generator. Then, `Click` on the input port (`left`) of the oscilloscope. Or simply click on the output port and drag (holding the click) to the input port of the next node.
3. **Configure (optional)**
   Click on a node, and an editor with the parameters of the selected node will appear in the left side panel to adjust operating conditions. At the bottom there is a visualization plot that visually represents the data generated or acquired by the nodes.
4. **Run**
   Press `F5` or the ▶ button on the toolbar. The topological engine will calculate the execution order, process the data and you will see the wave on the graph panel. The connector will animate indicating the active flow!


---

## 🧠 Understanding ports: categories by color

In FloWorks, each port belongs to a **functional category** identified by a color. Valid connections are **always made between ports of the same color**: an output from one category connects only to an input of the same category. In addition, the connector line automatically adopts the color of the ports it joins, making visual reading easier.

| Type | Color | Purpose | Typical example |
|------|-------|-----------|----------------|
| `control` | White  | Control flow / activation. | Start signal towards an acquisition node. |
| `exec` | Gray | Execution of operations or steps. | Triggering a function or callback. |
| `data` | Green  | Generic data / numeric signals. | Output from a generator or sensor. |
| `int` | Blue  | Integers. | Index, buffer size, ID. |
| `float` | Cyan  | Floating-point numbers. | Amplitude, frequency, threshold. |
| `string` | Purple  | Text strings. | File name, label. |
| `bool` | Pink  | Boolean values (`True`/`False`). | Status flag, enable. |
| `array` | Dark blue  | Arrays / vectors. | Multichannel signal, sample list. |
| `trigger` | Orange  | Triggers / discrete events. | Synchronization pulse, edge. |

**Golden rule:**

- Only ports of the **exact same color** are connected (output ↔ input of the same category).
- The system prevents invalid connections and visually highlights compatible ports when dragging.
- The connector line takes the color of the connected ports; this way each route is identified at a glance.

**FloWorks philosophy:**
Data ports **preserve the dimensionality** of arrays. Automatic flattening is never applied: if a matrix goes in, a matrix comes out, maintaining the integrity of your multidimensional signals.

![FloWorks](assets/tipos_de_puertos.PNG)

---

## 🖱️ Navigating the Canvas

Master the workspace with these gestures:

| Action | How to do it |
|--------|--------------|
| **Zoom** | Mouse wheel or `Ctrl + wheel` |
| **Pan (scroll)** | Hold `Space` and drag, or use the mouse middle button |
| **Select a node** | Left click on the node |
| **Multiple selection** | Drag a rectangle with left click, or `Ctrl + click` on several nodes |
| **Move selection** | Drag any of the selected nodes |
| **Open configuration** | Double-click on a node |

**Tip:** The left panel updates automatically with the configuration of the selected node, with no need to open additional windows.

---

## ⚡ Keyboard shortcuts and advanced moves

These shortcuts turn a normal user into a **power user**:

| Shortcut | Action |
|-------|--------|
| `F5` | Run flow |
| `Ctrl + S` | Save project (`.sflow`) |
| `Ctrl + Click` | Connect nodes (click on output port → click on input port) |
| `Ctrl + C` / `Ctrl + V` | Copy / paste selected nodes |
| `Ctrl + Z` / `Ctrl + Y` | Undo / redo |
| `Ctrl + Shift + L` | Auto-organize nodes on the canvas |
| `Del` | Delete selected nodes |
| `Ctrl + A` | Select all nodes |

**Advanced moves:**

- **Duplicate a flow:** select a group of nodes, `Ctrl + C`, `Ctrl + V` and drag the copy to another area.
- **Clean up the grid:** use `Ctrl + Shift + L` to tidy the whole canvas with a single command.
- **Quick connection:** `Ctrl + Click` on an output port and then normal click on the input port; FloWorks draws the connection automatically.

---

## 🎨 Environment customization

FloWorks adapts to you, not the other way around.

### Hot theme switching
From the top bar, **View → Theme** menu, choose between light, dark or others. The interface changes **instantly**, without restarting or losing your workflow.

### Font size
In **View → Font size** select a preset or a custom value. The whole interface adjusts immediately.

### Language
In **View → Language** select the desired language. FloWorks supports **hot switching**: menus, buttons and messages are translated without restarting the application.

---

## ❗ Troubleshooting common issues

| Problem | Possible cause | Solution |
|----------|---------------|----------|
| The flow does not run | There are unconfigured nodes or broken connections | Check that all nodes have valid parameters and that connections are between compatible ports |
| The plot does not update | The flow is paused or no data is flowing | Make sure you pressed `F5` or ▶, and that the source nodes are generating data |
| I cannot connect two nodes | The ports are of different type | Verify that both ports are **data** or both are **control** |
| The program slows down with large flows | Too many nodes or real-time plots | Close unused analysis panels or reduce the sampling frequency of source nodes |
| The theme does not change | Some widgets may not be registered | Restart the application and try again (this will be resolved in future versions) |

---

## 🧪 Quick practical examples

Besides the initial sine wave flow, try these mini-projects to master FloWorks:

| Example | Nodes involved | Expected result |
|---------|-------------------|--------------------|
| **Low-pass filter** | Generator → Filter → Graph Viewer | You will see the filtered signal |
| **Simulated acquisition** | Generator → THD Analyzer | THD value of the signal |
| **Manual control** | Generator → Data Inspector | Table with the values of the signal sent by the generator |
| **Signal comparison** | Two generators → Adder → Graph Viewer | The result of the operation (addition, subtraction, multiplication or division) of two waves in a single graph |

Each of these flows can be assembled in less than a minute, demonstrating FloWorks agility compared to traditional code.

---

## 📚 What's next?

| Resource | Description |
|---------|-------------|
| [🗺️ Main Interface Anatomy Guide](interface-anatomy.md) | Understanding the architecture and philosophy of the graphical interface |
| [🗺️ Code Map and Architecture](philosophy.md) | Complete structure, managers, contracts and DPI-Awareness. |
| [🧩 Technical Node Reference](node-reference.md) | Catalog, `ScriptNode`, multichannel and how to extend the system. |
| [🌐 Internationalization Guide](translation-guide.md) | Add languages, validate JSON and manage `tr()` keys. |
| [📦 Portable Build Guide](guia-ejecutable-portable.md) | PyInstaller, hooks, `--onefile`, error solving and digital signature. |

---

!!! warning "Compatibility and usage notes"
    1. **Python version:** You can use 3.9+, and 64-bit systems.
    2. **Windows Firewall:** If you use real hardware (VISA/SCPI oscilloscope), allow `FloWorks.exe` in the firewall. The app shows a custom dialog if the connection is blocked (the OS dialog does not appear in `--windowed` mode).
    3. **Key shortcuts:** `F5` (run), `Ctrl+S` (save `.sflow`), `Ctrl+Click` (connect), `Space+click` (free pan), `Ctrl+Shift+L` (auto-layout).
    4. **Data preservation:** The engine **never** applies `flatten()` to arrays. Work with local copies if you need to vectorize.
