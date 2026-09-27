---
title: FloWorks Architecture
description: Overview of components and internal operation for the end user
---

# FloWorks Architecture – User View

FloWorks is a desktop application that lets you build signal processing chains using flow diagrams. You connect blocks (nodes) on an interactive canvas and see the results in real time. To make this possible, the application is organized into several modules that work together. Below is a non-technical explanation of what each part does and how they relate to each other.

---

## Overall structure

The application is made up of the following functional areas:

| Area | What it does |
|------|------------|
| **Startup and main window** | Launches the program, shows the window, menus and coordinates all user actions. |
| **Execution engine** | Calculates the order in which nodes must run, detects dependencies and loops, and passes data from one node to another. |
| **Scene and diagram** | Manages the canvas where you place nodes, the connections between them, sticky notes and undo/redo actions. |
| **Nodes and processing** | Contains all the block types you can use: signal sources, mathematical operations, custom scripts, graph export, etc. |
| **Visual connectors** | Draws the lines that join nodes (smooth curves or orthogonal paths), animates them to show data flow and prevents overlap. |
| **User interface** | Includes the diagram view (zoom, pan), the toolbar, the parameter table, analysis panels (statistics, cursors) and configuration dialogs. |
| **Real hardware support** | Enables communication with laboratory instruments (oscilloscopes, generators, LCR meters) to capture or generate real signals. |
| **Graph export** | Generates high-quality images (PNG, PDF, SVG) with full visual customization. |
| **Themes and appearance** | Changes the look of the entire application (dark, light, high contrast) and lets you adjust font size. |
| **Languages** | Translates the whole interface into several languages and lets you switch language instantly. |
| **Project management** | Saves and opens `.sflow` files with the whole diagram, including configurations, scripts and results. |
| **Testing and diagnostics** | Internal tools to verify that everything works correctly (not visible to the end user). |

---

## How it works inside

### Startup and main window
When you open FloWorks, the graphical environment is set up, your screen pixel density is detected (so everything looks sharp on 4K or normal monitors) and the main window is shown. This window centralizes all elements: the drawing area, menus, toolbar and side panels.

### Flow engine
When you press "Run" (or press F5), an internal engine walks through all the nodes in the correct order, respecting the connections. It knows which nodes depend on others and prevents infinite loops. It supports a node receiving several named inputs and producing multiple outputs. Data travels between nodes without losing its original structure.

### Diagram scene
The canvas where you build your diagrams is a smart scene:

- Lets you add nodes, move them, connect them and select them.
- Supports unlimited undo and redo for any action.
- Includes resizable sticky notes that you can place freely and that are saved with the project.
- Has an automatic organizer that repositions nodes neatly (with Ctrl+Shift+L).
- When saving, the whole diagram is packaged into a `.sflow` file containing node descriptions, connections, notes and associated numerical data.

### Connectors
The lines that join nodes are drawn as smooth curves or orthogonal paths. A gentle animation of dots or dashes indicates the direction of flow. A lane manager prevents several connections between the same nodes from piling up; it separates them automatically so everything remains readable.

### Node types
Nodes are the fundamental pieces. They are grouped into three categories:

- **Sources** – Generate signals. They can simulate waves (sine, square, etc.) or read real data from a connected oscilloscope or multimeter. They support multiple simultaneous channels (for example, impedance and phase from an LCR).
- **Processing** – Transform the data. They include arithmetic operations (addition, subtraction, multiplication, division), conditional decisions (If/Else branching) and a powerful script node that lets you write your own Python code with visual aids.
- **Sinks** – Display or export the results. The most common is the graph viewer (virtual oscilloscope), but there is also a professional-quality graph exporter.

Each node has input ports (left/top) and output ports (right/bottom). When you connect an output port to an input port, the signal flows between them.

#### Advanced script node
The script node deserves special mention. It is intended for advanced users who want to add their own processing without leaving FloWorks. It offers:

- An editor with syntax highlighting, autocompletion and line numbering.
- The ability to define editable parameters from the node panel without touching the code (for example, a numeric value that is later used in the script).
- Dynamic input and output ports: by adding special comments in the script, you can create new connectors.
- Persistent memory: a special variable (`persist`) that keeps its value between runs, useful for accumulators or state machines.
- Ready-made script templates and the option to save your own.
- A built-in help system and a console that shows runtime errors.

### User interface
Besides the canvas, the interface includes:

- A **toolbar** with all nodes organized by category, language, theme and font size menus, and access to the log viewer.
- A **parameter table** that shows information about selected nodes and highlights possible incompatibilities (such as trying to operate on signals of different lengths).
- **Dockable analysis panels**: statistics (maximum, minimum, RMS), A/B cursors to measure differences, and a crosshair with peak marker.
- A **welcome dialog** that adapts to your screen resolution and offers initial options.

### Connection with real instruments
If you have compatible hardware (Siglent SDS oscilloscopes, LCR meters, SDG generators), FloWorks can communicate with them via the standard VISA/SCPI protocol. Configuration is done from specific panels inside the application. When you capture a multichannel signal (for example, magnitude and phase from an LCR), the source node packages all channels and you can choose which one to view with a simple context menu.

### Professional graph export
The graph exporter node lets you generate images ready for reports or publications. Double-clicking it opens a dialog with multiple options: you can customize colors, line types, labels, scales, choose between PNG, PDF or SVG formats, and save your preferences as reusable profiles.

### Visual customization
FloWorks includes several themes (dark, light, high contrast) that change the appearance of the entire interface instantly, without restarting. In addition, you can adjust the global font size from the menu (Info → Font size) and all elements are resized accordingly, including texts inside nodes, sticky notes and graphs.

### Language system
The application automatically detects your system language on first launch and saves your preference. You can change the language at any time from the menu; all texts, menus and help are updated on the fly.

### Projects and `.sflow` files
All your work is saved in a single file with the `.sflow` extension. This file contains the complete diagram: nodes, connections, notes, configurations, scripts and generated numerical data. You can share it with other users; when opened on another computer, notes and nodes are automatically rescaled to match that screen's pixel density.

---

## Typical workflows

1. **Create a simple diagram**
   Select a source node (e.g., Generator) and a Viewer node from the toolbar.
   Connect the generator output to the viewer input (Ctrl+click on the output port, then click on the input port).
   Press F5 to run. You will see the signal on the graph.

2. **Use a custom script**
   Add a Script node.
   Write your Python code in the editor; you can define editable parameters and extra ports.
   Connect its inputs and outputs like any other node.
   Run the flow; the script is processed with your data.

3. **Capture data from a real oscilloscope**
   Connect the instrument and configure communication from the Oscilloscope node panel.
   The node acquires the signal and delivers it through its output ports (one per channel).
   Connect those ports to other processing nodes or to the viewer.

4. **Export a graph for a report**
   Connect the desired signal to a Graph Exporter node.
   Select the node (right-click) to configure the visual appearance of the graph.
   Profiles can also be loaded/saved to speed up obtaining report-ready graphs by getting the image file in the chosen extension.

---

## What all this is for

This architecture is designed so you can concentrate on signal analysis without worrying about how the program is organized internally. Each component has a clear function and works together to provide a smooth experience, from simulation to real instrumentation, through visual customization and result export.

If you ever need to extend FloWorks capabilities (for example, by adding new node types or connecting a different instrument), know that there is a modular structure that allows it, although that is developer territory. As an end user, enjoy the flexibility this design provides.
