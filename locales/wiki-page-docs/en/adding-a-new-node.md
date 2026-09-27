---
title: Guide for Adding a New Node to FloWorks
description: Step-by-step tutorial for creating, registering and integrating custom nodes into the FloWorks flow engine.
---

# 📘 Developer Guide: How to Add a New Node to FloWorks

This guide describes the complete process for creating a new node type in FloWorks, ensuring that it integrates correctly with the flow engine, the user interface, visual themes and the internationalization system.

---

## 📋 Table of Contents
- [📘 Developer Guide: How to Add a New Node to FloWorks](#-developer-guide-how-to-add-a-new-node-to-floworks)
  - [📋 Table of Contents](#-table-of-contents)
  - [1. Introduction to the Architecture](#1-introduction-to-the-architecture)
  - [2. Using the `template_node.py` Template](#2-using-the-template_nodepy-template)
  - [3. Step by Step: Creating a Custom Node](#3-step-by-step-creating-a-custom-node)
    - [3.1. Copy and Rename the Template](#31-copy-and-rename-the-template)
    - [3.2. Define Ports and Labels](#32-define-ports-and-labels)
    - [3.3. Implement the Processing Logic](#33-implement-the-processing-logic)
    - [3.4. Customize the Appearance (Optional)](#34-customize-the-appearance-optional)
    - [3.5. Add Configurable Parameters (Optional)](#35-add-configurable-parameters-optional)
    - [3.6. Make the Node Serializable (save / load configurations)](#36-make-the-node-serializable-save--load-configurations)
  - [4. Integration into the System](#4-integration-into-the-system)
  - [5. Internationalization (i18n)](#5-internationalization-i18n)
  - [6. Visual Themes](#6-visual-themes)
  - [7. Checklist and Troubleshooting](#7-checklist-and-troubleshooting)
    - [✅ Checklist](#-checklist)
    - [🐛 Common Issues](#-common-issues)
  - [8. Conclusion](#8-conclusion)

---

## 1. Introduction to the Architecture

FloWorks is built on PySide6 and uses a model of connectable nodes that represent a signal processing flow.

---

## 2. Using the `template_node.py` Template

To facilitate the creation of new nodes, the file `nodes/template_node.py` is provided. This template includes:
- Full support for internationalization (connection to `languageChanged`, `update_language` method).
- Full support for themes (`update_theme` method).
- Built-in help with a three-section HTML format.
- Management of multiple configurable input/output ports.
- Multiple outputs with `get_output_for_port`.
- Visualization in the plot via `get_display_signal`.
- Translatable context menu.

It is recommended to always start from this template when developing a new node.

---

## 3. Step by Step: Creating a Custom Node

### 3.1. Copy and Rename the Template
1. Copy `nodes/template_node.py` with the name of your new node, for example `nodes/my_node.py`.
2. Rename the class from `TemplateNode` to something descriptive, e.g. `MyNodeNode`.
3. Adjust the imports if necessary.

### 3.2. Define Ports and Labels
!!! warning "Important: Name Matching"
    The port names in `PORTS`, `PORT_LABELS` and the keys of the dictionary returned by `execute_program` must be **exactly the same** (including uppercase/lowercase). The template now includes an alias mapping (`'data_in'` → first left port) for greater robustness.

Edit the `PORTS` dictionary at the top of the file. Each entry has the format:
```python
"port_name": ("side", fraction)
```
- **Possible sides:** `"left"`, `"right"`, `"top"`, `"bottom"`.
- **Fraction:** value between `0.0` and `1.0` indicating the position along the side.

**Example for a node with one input and two outputs:**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
The `PORT_LABELS` dictionary contains the text that will appear next to each port. It is recommended to use translation keys instead of fixed text (see Internationalization section).

### 3.3. Implement the Processing Logic
The key method is `execute_program(self, input_data)`. This method is invoked by the flow engine when the node receives data.

**`input_data` can be:**
- `None` if there is no input.
- A tuple `(x, y)` for time signals.
- A 1D array.
- A dictionary `{port_name: data}` in nodes with multiple inputs.

**Return value:**
- For nodes with a single output, return the data directly (e.g. tuple `(x, y)`).
- For nodes with multiple outputs, return a dictionary where the keys match the names of the output ports defined in `PORTS`.

```python
def execute_program(self, input_data):
    # Process input_data and generate results
    magnitude_result = (freq, mag)
    phase_result = (freq, phase)
    return {
        "magnitude": magnitude_result,
        "phase": phase_result
    }
```

!!! tip "Note on generic port names"
    The flow engine may occasionally pass a dictionary with keys like `'data_in'` instead of the real port name (especially if the user did not click exactly on the circle). The template already includes code to handle this case:
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    This prevents the node from failing due to an imprecise connection error.

The template already includes a commented example. It also implements `get_output_for_port(self, port_name)` so that the engine can route each output:
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. Customize the Appearance (Optional)
The `paint()` method draws the background, title, status and any additional text. You can modify:
- The colors (they are automatically updated with `update_theme`).
- The status text (using the `self._status` attribute).
- Summary information (e.g. magnitude peak).

The template shows a basic example.

### 3.5. Add Configurable Parameters (Optional)
If your node requires user-adjustable parameters (e.g. window size, cutoff frequency), you can:
1. Add attributes in `__init__` (e.g. `self.window_size = 512`).
2. Create a configuration dialog (inherits from `QDialog`).
3. Connect the dialog in `open_config_dialog()` (method already present in the template).
4. Update the parameters from the dialog and call `self.update()`.

### 3.6. Make the Node Serializable (save / load configurations)
So that the node can save and recover its parameters when copying/pasting, undoing/redoing, or when using the Save/Open commands from the File menu, it must inherit from the serialization mixin and declare its attributes.

1. Import the mixin in your file:
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. Change the class inheritance to include it before `QGraphicsObject`:
    ```python
    class MyNodeNode(SerializableMixin, QGraphicsObject):
    ```
3. Define the `SERIALISABLE` list at class level, with the names of the attributes you want to persist. Only simple types (`int`, `float`, `str`, `bool`), lists, dictionaries, or NumPy arrays are supported (the latter are automatically stored as `.npy` files inside the `.sflow`).
    ```python
    class MyNodeNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frequency', 'amplitude', 'configuration']
    ```
4. Make sure these attributes are initialized in `__init__`:
    ```python
    self.frequency = 1000.0
    self.amplitude = 1.0
    self.configuration = {'type': 'sine', 'phase': 0}
    ```

With this, you do not need to write `serialize`/`deserialize` methods; the mixin automatically takes care of saving and recovering the values.

If your node requires additional logic when loading (for example, reconnecting a hardware instrument), you can override `deserialize` by first calling the parent method:
```python
def deserialize(self, data):
    super().deserialize(data)   # restores SERIALISABLE attributes
    self._start_device()
```

---

## 4. Integration into the System

Once the node file is created, you only need to drop it into the `nodes` folder for it to appear in the interface and work with the rest of the system.

---

## 5. Internationalization (i18n)

All visible texts must be translatable via `tr("key", default="...")`. The template already implements this. You must add the corresponding keys in the JSON files inside `locales/`.

**Recommended structure:**
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

The HTML help follows the three-section format common to all nodes (specific description + "How to think about the system" + "Shortcuts and tricks"). The template already includes the structure in `get_help_text()`.

---

## 6. Visual Themes

The `update_theme(self, theme)` method receives a dictionary with the colors defined by the current theme. The template automatically updates:
- Node background (`node_normal_bg`)
- Border (`node_selected_border`)
- Title and text color (`node_normal_text`)
- Port colors (`port_circle`, `port_outline`, `port_inline`, `port_text`)

Make sure that in `MainWindow` (or `ThemeUpdater`) `node.update_theme()` is called for each node when the theme changes.

---

## 7. Checklist and Troubleshooting

### ✅ Checklist
- [ ] The node is created correctly from the toolbar.
- [ ] The ports are displayed in the expected positions and are detectable for connections (`Ctrl+click`).
- [ ] When receiving input data, `execute_program` is called and the signal is processed.
- [ ] The outputs propagate correctly to connected nodes.
- [ ] The context menu allows changing the display channel (if there are multiple outputs).
- [ ] When clicking on the node, the selected signal is plotted in the plot widget.
- [ ] Double-click opens the help with the proper format.
- [ ] The language changes correctly (title texts, ports, menus).
- [ ] The theme changes correctly (node and port colors).
- [ ] Copy/paste works without errors.

!!! tip "Precise port connections"
    When connecting nodes, make sure to click exactly on the target port circle. If you click on the node body, the system will use a generic name (`'data_in'`). The template now tolerates these names, but it is good practice to connect directly to the circle to guarantee correct routing of multiple outputs.

### 🐛 Common Issues

| Symptom | Possible Cause | Solution |
|---------|---------------|----------|
| The connection arrow does not anchor to the port. | The port circle does not have `setData(0, port_name)` or `get_port_scene_pos` is not implemented. | Verify that in `_create_ports` `circle.setData(0, port_name)` is done and that `get_port_scene_pos` uses that name. |
| The outputs do not reach the connected nodes. | `execute_program` does not return a dictionary (for multiple outputs) or `get_output_for_port` is not implemented. | Ensure that `execute_program` returns `{port_name: data}` and that `get_output_for_port` returns the corresponding value. |
| When clicking on the node nothing is plotted. | `get_display_signal` does not return a valid `(x, y)` tuple or `display_channel` does not match an existing output. | Check that `get_display_signal` uses the selected channel and that the data are NumPy arrays. |
| Texts do not update when changing language. | The `languageChanged` signal was not connected or `update_language` does not update the elements. | Verify the connection in `__init__`: `language_manager.languageChanged.connect(self.update_language)`. |
| The theme is not applied. | `update_theme` is not called when creating the node or when changing theme. | In `MainWindow`, after creating the node, invoke `node.update_theme(self.theme_manager.current_theme())`. |
| The arrow points to the center of the node. | The body was clicked instead of the circle, or the name does not match `PORTS`. | Click directly on the circle. Verify that `get_port_scene_pos` has the alias mapping. |
| `NameError: name 'self' is not defined` on import. | Instance attributes were declared outside `__init__`. | All attributes like `self.my_parameter` must be defined inside `__init__`. |
| Parameters are lost when copying/opening `.sflow`. | The node does not inherit from `SerializableMixin` or did not define `SERIALISABLE`. | Implement step 3.6 of this guide. |

---

## 8. Conclusion

By following this guide and using the `template_node.py` template, you will be able to add new nodes to FloWorks efficiently and consistently with the rest of the system. Always remember to maintain compatibility with i18n and themes for a professional user experience.

Feel free to contribute your own nodes!
