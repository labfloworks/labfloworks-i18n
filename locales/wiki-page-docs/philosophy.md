# Philosophy

## Overview

FloWorks is a desktop application that allows you to create signal processing chains through visual flow diagrams.
Drag, connect and configure nodes; the result is calculated and displayed in real time.
Work with simulated signals or connect real instruments (oscilloscopes, generators, LCR multimeters) without needing to write code, although a powerful scripting environment is available if you wish to extend functionality.

---

## Main features

- **Interactive diagrams** – Build your workflow by joining nodes with lines that represent data flow.
- **Real-time processing** – Every change is immediately reflected in plots and visualizations.
- **Simulation and real hardware** – Generate test signals or capture data directly from laboratory instruments.
- **Advanced script node** – Incorporate your own Python code with autocompletion help, editable dynamic parameters and persistent memory between runs.
- **Professional visualization** – Signals, spectra, spectrograms and high-quality graphs ready to export.
- **Multilingual** – The interface detects the system language and allows switching between Spanish, English and other languages at any time.
- **Visual themes** – Dark, light and high-contrast modes to suit your preferences or accessibility needs.
- **Complete project management** – Save your work in `.sflow` files and recover it exactly as you left it, with unlimited undo and redo.

---

## How to work with FloWorks

### Nodes
A node is a piece of processing. They are organized into three categories:

- **Sources** – Insert signals at the beginning of the flow. For example, an oscilloscope (real or simulated), a function generator or a mathematical operation.
- **Processing** – Transform the data. Additions, subtractions, conditionals, filters… even a special node for writing your own Python scripts.
- **Sinks** – Display or export the results. The graph viewer and the professional graph exporter are the most used.

### Connections
The unions between nodes are drawn as smooth curves or orthogonal lines. A flow animation indicates the direction of data at all times. The system automatically organizes cables so they do not overlap.

### Visualization
Whenever a node produces a signal, it can be seen in the integrated plot panel. You can explore different representations (waveform, spectrum, spectrogram) and adjust the scale with the mouse.

---

## Highlighted nodes
These are the minimum essential nodes needed for the program's philosophy to make sense.

### Signal generator node
Source of signals that can generate custom simulations of waveforms to the user's taste. It allows selecting or typing the required waveform from a context menu.

### Script node
A complete programming environment within the diagram:

- **Editor with syntax highlighting**, autocompletion and error console.
- **Dynamic parameters** – Define editable variables from the node panel without modifying the code.
- **Configurable ports** – Add additional inputs and outputs directly from the editor.
- **Persistent state** – Save values between runs; everything is stored together with the project.

### Graph exporter
Sink node that generates high-quality images for reports or publications. Allows configuring size, resolution, format, among others.

---

## Customization

- **Language** – The application automatically detects the system language and saves your preference. You can change it from the menu without restarting.
- **Appearance** – Choose between dark, light or high-contrast theme according to ambient light or your visual needs.

---

## Projects and files

Save your complete diagram in a `.sflow` file.
When opening it you will recover all nodes, connections, scripts, parameters and visualization settings.
Undo and redo actions let you experiment without fear of losing previous work.

---

FloWorks is designed so you can concentrate on signal analysis and not on the technical details of implementation. Drag, connect and discover.
