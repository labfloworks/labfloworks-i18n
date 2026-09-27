# 🧪 FloWorks Console Interactive Tutorial

Welcome to the FloWorks experimentation lab. This section is for more advanced users; it is a **Python** terminal to control everything related to the canvas, that is, nodes and their connections, sequentially and line by line. It is a terminal linked to the program that can control it and determine behaviors or routines for the benefit of more demanding users.

This guide will show you **step by step** how to control and analyse your flow diagrams without touching the mouse. Every example has been validated in the interactive console and reflects the program's real data structure.

---

## 1. Knowing the terrain

The console injects three global objects: `app` (main window), `graph` (scene/diagram) and `selected_node` (node currently selected on the canvas). All commands start from these three.

### View all nodes

```python
>>> graph.nodes
```

**Example output:**
```
Nodes in scene:
  [0] Advanced Signal Generator (type: SignalSourceNode, category: Sources)
  [1] FFT (type: FFTNode, category: Processing)
```

The index in brackets (`[0]`, `[1]`) is your main way to access a node. The order is the creation order on the canvas.

#### Alternative: count nodes or filter by type

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### View all connections

```python
>>> graph.connections
```

**Example output:**
```
Connections in scene:
  [0] Advanced Signal Generator (out) → FFT (input)
```

The output shows the source node name, the output port, the arrow, the destination node and the input port. If a connection does not appear, the flow cannot be executed.

#### Alternative: view connections of a single node

```python
>>> selected_node.connectors
```

### View the selected node

Click a node on the canvas and then run:

```python
>>> selected_node
```

**Example output:**
```
Node: FFT
  Type: FFTNode
  Category: Processing
  Ports: ['input', 'output', 'magnitude', 'phase']
```

> **💡 Note:** If no node is selected, `selected_node` is `None`. Selecting a node also updates the side parameter table automatically.

#### Alternative: select a node by code

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. Manipulate nodes and connections without the mouse

### Create a new node

You must know the exact class name of the node (same as in the catalogue). The arguments are: `(type, x, y)`.

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

The node appears on the canvas at coordinates (300, 200). If you do not know the exact name, list the categories (see section 7).

#### Alternative: create several nodes at once

```python
>>> for i, tipo in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(tipo, 100 + i*200, 300)
```

### Connect nodes manually

Syntax: `graph.connect_nodes(source, destination, 'output_port', 'input_port')`. Ports depend on each node; never assume their names.

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 Note:** Always check `graph.nodes[N].PORTS` before connecting. An FFT node has `'input'` and `'magnitude'`; a generator has `'output'`.

#### Alternative: connect to the default port

If you do not know the exact input port name, some nodes accept `None` to use the first available one:

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### Delete a node

```python
>>> graph.remove_node(graph.nodes[2])
```

Removes the node and all its associated connections. The indices of `graph.nodes` are reordered, so do not keep old references.

#### Alternative: delete all nodes of a category

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### View the ports of a node

```python
>>> graph.nodes[1].PORTS
```

**Example output:**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

The key is the port name (string). The value is a tuple with the visual position. Only the keys interest you for connecting.

#### Alternative: view ports as a simple list

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. Execute the flow and view results

### Execute the whole graph

```python
>>> graph.execute_flow()
```

This method belongs to the diagram (`graph`), not the main window. It traverses all nodes in topological order, executes each one and caches the results. It returns nothing; data is stored internally.

#### Alternative: force calculation of a specific branch

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

This recalculates the entire upstream tree of the indicated node and returns the result directly, without modifying the global cache.

### View cached data of a node

If you need to access the processed data of a specific node, you can do it in two ways:

#### Direct way (by object)
```python
>>> graph.node_values[graph.nodes[1]]
```

**Example output:**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ Warning:** This way may fail with `KeyError` if the order of the nodes in the scene changed (for example, by deleting or adding nodes) or if the object instance does not exactly match the key stored in the dictionary.

#### Alternative way (by position)
```python
>>> list(graph.node_values.values())[1]
```

This way is **more stable** because it does not depend on the exact object identity. The order of the values follows the sequence in which the nodes were executed during the last `graph.execute_flow()`. The index `[1]` corresponds to the second node in that sequence.

> **💡 Note:** If you want to see the index of each node in the execution order, you can use:
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ Caution:** `graph.node_values` does not always return an array directly. For processor nodes (FFT, filters, etc.) it returns a **dictionary** where each key is an output port. For source nodes, it returns a tuple `(x, y)`.

#### Alternative: view data of all nodes in one line
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### Access the Y axis of a source node

Generator nodes (SignalSourceNode, FileInputNode, etc.) return a tuple `(time, signal)` when executed. To get only the Y axis:

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**Example output:**
```
Scalar NumPy (float64): 1.0
```

#### Alternative: get the X axis (time)

```python
>>> x = result[0]
>>> x[:5]
```

### Python assignments: a vital detail

In Python, assignments (`=`) are **statements**, not expressions. The console prints nothing after `x, y = ...` because there is no return value.

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

To verify it worked, evaluate the variable on the next line:

```python
>>> x
>>> y.shape
```

Or use `;` to chain an expression on the same line:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

Or use explicit `print()`:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. Draw on the main plot

### Clear the plot

```python
>>> app.plot_widget.clear_plot()
```

#### Alternative: clear and redraw immediately

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### Draw an arbitrary signal from the console

You can create arrays with NumPy and send them directly to the plotting widget, without going through any node.

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### Alternative: draw a sum of sines

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### Draw the result of a source node

Since the source node returns `(x, y)`, you can unpack directly:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### Draw the result of an FFT node (multiple ports)

Nodes with multiple outputs (FFT, time-frequency analysis, etc.) do not return a simple tuple. They return a `dict` where each key is an output port.

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**Example output:**
```
dict_keys(['output', 'magnitude', 'phase'])
```

Notice that `'output'` may be `None` if the node does not have a generic port. The useful outputs are `'magnitude'` and `'phase'`, which in turn are tuples `(frequencies, values)`:

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### Alternative: draw the phase instead of the magnitude

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### Alternative: overlay two signals

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # another node
>>> app.plot_widget.plot_waveform(x2, y2)  # overlays
```

> **💡 Note:** If you try to do `x, y = result` with a dict, Python will raise `ValueError: too many values to unpack`. Always inspect with `type(result)` and `result.keys()` before unpacking.

---

## 5. Modify the application on the fly

### Update the side parameter table

If you modify a parameter by code and want the side table to reflect the change:

```python
>>> app.workspace_table.populate()
```

This method takes no arguments. It refreshes the table with the current values of the selected node.

#### Alternative: force selection of another node and refresh

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### Add a node from the toolbar

```python
>>> app.add_node('SignalSourceNode')
```

It is equivalent to pressing the "+" button on the toolbar. The node is placed at a default position on the canvas.

### Change the window title

`setWindowTitle` is Qt's native method. It works, but keep in mind that the application may have a timer or event that calls `update_title()` and overwrites it automatically.

```python
>>> app.setWindowTitle('My signal laboratory')
>>> app.windowTitle()
```

To restore the "official" title that the app calculates from its internal state (project name, file, etc.):

```python
>>> app.update_title()
```

#### Alternative: title with project name

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. Navigation and productivity in the console

The console is not a simple `print()`. It has history, autocompletion and multiline blocks.

| Key / Command             | Action                                                         |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | Navigate through executed command history                      |
| `Tab`                     | Autocomplete variables, attributes and methods in the namespace|
| `Ctrl + L`                | Clear the whole console (deletes text, not Python state)       |
| `if`, `for`, `def`, `class` | Prompt changes from `>>>` to `...` for multiline blocks       |
| `Ctrl+C` (on selection)   | Copy text from the console                                     |
| `Ctrl+A`                  | Select all content                                             |

> **💡 Note:** Autocompletion uses `rlcompleter` and recognises the entire injected namespace (`app`, `graph`, `selected_node`) plus any variable you define in the session.

---

## 7. Advanced recipes

### Change an internal parameter of a node

Node parameters are not flat attributes. They are nested inside the `params` dictionary, which in turn has subsections such as `'preset'`, `'formula'` or `'advanced'`. Never do `node.amplitude = 3.0`; that creates a new attribute on the object but does not modify the real parameter.

#### Case A: modify a preset (sine, square, etc.)

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Case B: use a custom formula

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

The expression uses `t` as the time variable. The values in `vars` are the symbols you can reference in the formula. If you omit `vars`, the node will use default values and the formula may not reflect the change.

#### Case C: change advanced parameters (sample rate, duration)

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Case D: change parameter of a non-generator node (e.g. FFT)

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 Note:** `getattr(obj, '_generate_signal', lambda: None)()` is a safe pattern: if the method exists (generator nodes), it calls it; if not, it does nothing and does not raise an error. For processor nodes, only `graph.execute_flow()` is enough.

### List all available node categories

The import loads the catalogue, but does not display it automatically. Remember that a successful import in Python prints nothing; you must evaluate the object.

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

To see a readable summary:

```python
>>> for cat, nodes in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(nodes)} nodes")
```

#### Alternative: list node names by category

```python
>>> {cat: [n.__name__ for n in nodes] for cat, nodes in NODE_CATEGORIES.items()}
```

### View help for any method

```python
>>> help(graph.connect_nodes)
```

The docstring appears directly in the console. It is useful for discovering what arguments a method expects without opening the source code.

#### Alternative: view filtered attributes

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. What to do if something goes wrong?

- **Red error in the console:** the full traceback is shown. The application does not close; you can correct the command and try again.
- **Interface freezes:** you probably wrote an infinite loop. The console runs in a separate thread, but if the loop affects the GUI thread, restart the application.
- **Unexpected `None`:** if a node returns `None` instead of data, verify that it is connected upstream (`graph.connections`) and that the flow has been executed (`graph.execute_flow()`).
- **`ValueError: too many values to unpack`:** you are trying to unpack a dict as if it were a tuple. Use `result.keys()` first.
- **`ValueError: not enough values to unpack`:** you expect 2 values but the node returns 1 (dict) or 3 (spectrogram). Inspect with `type(result)` before unpacking.
- **`AttributeError`:** the object does not have that attribute. Use `dir(obj)` or `[a for a in dir(obj) if 'word' in a.lower()]` to discover the correct name.
- **Nothing happens when executing:** check that there is at least one source node connected to the chain and that `graph.execute_flow()` has been called. Processor nodes do not generate data on their own.
- **The plot does not change:** make sure to call `graph.execute_flow()` after modifying parameters. Only changing `params` does not recalculate automatically.

---

© 2026 FloWorks — Signal Laboratory
