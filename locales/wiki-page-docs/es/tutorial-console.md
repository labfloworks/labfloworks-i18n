# 🧪 Tutorial Interactivo de la Consola FloWorks

Bienvenido al laboratorio de experimentación de FloWorks. Esta es una sección para usuarios mas avanzados, se trata de una terminal **Python** para controlar todo lo asociado al lienzo, vamos, nodos y sus conexiones, de forma secuencial y por líneas de código, se trata de una terminal enlazada al programa que lo puede controlar y determinar comportamientos o rutinas para facilidad de usuarios mas exigentes.

Esta guía te mostrará **paso a paso** cómo controlar y analizar tus diagramas de flujo sin tocar el ratón. Cada ejemplo fue validado en la consola interactiva y refleja la estructura real de datos del programa.

---

## 1. Conocer el terreno

La consola inyecta tres objetos globales: `app` (ventana principal), `graph` (escena/diagrama) y `selected_node` (nodo actualmente seleccionado en el lienzo). Todos los comandos parten de estos tres.

### Ver todos los nodos

```python
>>> graph.nodes
```

**Salida de ejemplo:**
```
Nodos en la escena:
  [0] Generador de Señales Avanzado (tipo: SignalSourceNode, categoría: Sources)
  [1] FFT (tipo: FFTNode, categoría: Processing)
```

El índice entre corchetes (`[0]`, `[1]`) es tu forma principal de acceder a un nodo. El orden es el de creación en el lienzo.

#### Alternativa: contar nodos o filtrar por tipo

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### Ver todas las conexiones

```python
>>> graph.connections
```

**Salida de ejemplo:**
```
Conexiones en la escena:
  [0] Generador de Señales Avanzado (out) → FFT (input)
```

La salida muestra el nombre del nodo origen, el puerto de salida, la flecha, el nodo destino y el puerto de entrada. Si una conexión no aparece, el flujo no podrá ejecutarse.

#### Alternativa: ver conexiones de un solo nodo

```python
>>> selected_node.connectors
```

### Ver el nodo seleccionado

Haz clic en un nodo del lienzo y luego ejecuta:

```python
>>> selected_node
```

**Salida de ejemplo:**
```
Nodo: FFT
  Tipo: FFTNode
  Categoría: Processing
  Puertos: ['input', 'output', 'magnitude', 'phase']
```

> **💡 Nota:** Si no hay ningún nodo seleccionado, `selected_node` vale `None`. Seleccionar un nodo también actualiza la tabla de parámetros lateral automáticamente.

#### Alternativa: seleccionar un nodo por código

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. Manipular nodos y conexiones sin ratón

### Crear un nodo nuevo

Debes conocer el nombre exacto de la clase del nodo (igual que en el catálogo). Los argumentos son: `(tipo, x, y)`.

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

El nodo aparece en el lienzo en las coordenadas (300, 200). Si no conoces el nombre exacto, lista las categorías (ver sección 7).

#### Alternativa: crear varios nodos a la vez

```python
>>> for i, tipo in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(tipo, 100 + i*200, 300)
```

### Conectar nodos manualmente

Sintaxis: `graph.connect_nodes(origen, destino, 'puerto_salida', 'puerto_entrada')`. Los puertos dependen de cada nodo; nunca asumas los nombres.

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 Nota:** Siempre revisá `graph.nodes[N].PORTS` antes de conectar. Un nodo FFT tiene `'input'` y `'magnitude'`; un generador tiene `'output'`.

#### Alternativa: conectar al puerto por defecto

Si no sabes el nombre exacto del puerto de entrada, algunos nodos aceptan `None` para usar el primero disponible:

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### Eliminar un nodo

```python
>>> graph.remove_node(graph.nodes[2])
```

Elimina el nodo y todas sus conexiones asociadas. Los índices de `graph.nodes` se reordenan, así que no guardes referencias viejas.

#### Alternativa: eliminar todos los nodos de una categoría

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### Ver los puertos de un nodo

```python
>>> graph.nodes[1].PORTS
```

**Salida de ejemplo:**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

La clave es el nombre del puerto (string). El valor es una tupla con la posición visual. Solo las claves te interesan para conectar.

#### Alternativa: ver puertos como lista simple

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. Ejecutar el flujo y ver resultados

### Ejecutar todo el grafo

```python
>>> graph.execute_flow()
```

Este método pertenece al diagrama (`graph`), no a la ventana principal. Recorre todos los nodos en orden topológico, ejecuta cada uno y cachea los resultados. No devuelve nada; los datos quedan almacenados internamente.

#### Alternativa: forzar cálculo de una rama específica

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

Esto recalcula todo el árbol aguas arriba del nodo indicado y devuelve el resultado directamente, sin modificar el cache global.

### Ver los datos cacheados de un nodo

Si necesitas acceder a los datos procesados de un nodo específico, puedes hacerlo de dos formas:

#### Forma directa (por objeto)
```python
>>> graph.node_values[graph.nodes[1]]
```

**Salida de ejemplo:**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ Advertencia:** Esta forma puede fallar con `KeyError` si el orden de los nodos en la escena cambió (por ejemplo, al eliminar o agregar nodos) o si la instancia del objeto no coincide exactamente con la clave guardada en el diccionario.

#### Forma alternativa (por posición)
```python
>>> list(graph.node_values.values())[1]
```

Esta forma es **más estable** porque no depende de la identidad exacta del objeto. El orden de los valores sigue la secuencia en que los nodos fueron ejecutados durante el último `graph.execute_flow()`. El índice `[1]` corresponde al segundo nodo en esa secuencia.

> **💡 Nota:** Si quieres ver el índice de cada nodo en el orden de ejecución, puedes usar:
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ Cuidado:** `graph.node_values` no siempre devuelve un array directo. Para nodos procesadores (FFT, filtros, etc.) devuelve un **diccionario** donde cada clave es un puerto de salida. Para nodos fuente, devuelve una tupla `(x, y)`.

#### Alternativa: ver datos de todos los nodos en una línea
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### Acceder al eje Y de un nodo fuente

Los nodos generadores (SignalSourceNode, FileInputNode, etc.) devuelven una tupla `(tiempo, señal)` al ejecutarse. Para obtener solo el eje Y:

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**Salida de ejemplo:**
```
Scalar NumPy (float64): 1.0
```

#### Alternativa: obtener el eje X (tiempo)

```python
>>> x = result[0]
>>> x[:5]
```

### Asignaciones en Python: un detalle vital

En Python, las asignaciones (`=`) son **statements**, no expresiones. La consola no imprime nada después de `x, y = ...` porque no hay valor de retorno.

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

Para verificar que funcionó, evaluá la variable en la línea siguiente:

```python
>>> x
>>> y.shape
```

O usá `;` para encadenar una expresión en la misma línea:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

O usá `print()` explícito:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. Dibujar en la gráfica principal

### Limpiar la gráfica

```python
>>> app.plot_widget.clear_plot()
```

#### Alternativa: limpiar y redibujar inmediatamente

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### Dibujar una señal arbitraria desde la consola

Podés crear arrays con NumPy y enviarlos directamente al widget de ploteo, sin pasar por ningún nodo.

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### Alternativa: dibujar una suma de senos

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### Dibujar el resultado de un nodo fuente

Como el nodo fuente devuelve `(x, y)`, podés desempaquetar directamente:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### Dibujar el resultado de un nodo FFT (puertos múltiples)

Los nodos con múltiples salidas (FFT, análisis tiempo-frecuencia, etc.) no devuelven una tupla simple. Devuelven un `dict` donde cada clave es un puerto de salida.

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**Salida de ejemplo:**
```
dict_keys(['output', 'magnitude', 'phase'])
```

Observá que `'output'` puede ser `None` si el nodo no tiene un puerto genérico. Las salidas útiles son `'magnitude'` y `'phase'`, que a su vez son tuplas `(frecuencias, valores)`:

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### Alternativa: dibujar la fase en lugar de la magnitud

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### Alternativa: superponer dos señales

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # otro nodo
>>> app.plot_widget.plot_waveform(x2, y2)  # se superpone
```

> **💡 Nota:** Si intentás hacer `x, y = result` con un dict, Python lanzará `ValueError: too many values to unpack`. Siempre inspeccioná con `type(result)` y `result.keys()` antes de desempaquetar.

---

## 5. Modificar la aplicación sobre la marcha

### Actualizar la tabla de parámetros lateral

Si modificás un parámetro por código y querés que la tabla lateral refleje el cambio:

```python
>>> app.workspace_table.populate()
```

Este método no recibe argumentos. Refresca la tabla con los valores actuales del nodo seleccionado.

#### Alternativa: forzar selección de otro nodo y refrescar

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### Añadir un nodo desde la barra de herramientas

```python
>>> app.add_node('SignalSourceNode')
```

Es equivalente a pulsar el botón "+" en la barra de herramientas. El nodo se coloca en una posición predeterminada del lienzo.

### Cambiar el título de la ventana

`setWindowTitle` es el método nativo de Qt. Funciona, pero tené en cuenta que la aplicación puede tener un timer o evento que llame a `update_title()` y te lo sobrescriba automáticamente.

```python
>>> app.setWindowTitle('Mi laboratorio de señales')
>>> app.windowTitle()
```

Para restaurar el título "oficial" que la app calcula desde su estado interno (nombre de proyecto, archivo, etc.):

```python
>>> app.update_title()
```

#### Alternativa: título con nombre de proyecto

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. Navegación y productividad en la consola

La consola no es un simple `print()`. Tiene historial, autocompletado y bloques multilínea.

| Tecla / Comando           | Acción                                                         |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | Navegar por el historial de comandos ejecutados                |
| `Tab`                     | Autocompletar variables, atributos y métodos del namespace     |
| `Ctrl + L`                | Limpiar toda la consola (borra texto, no el estado de Python)  |
| `if`, `for`, `def`, `class` | El prompt cambia de `>>>` a `...` para bloques multilínea     |
| `Ctrl+C` (en selección)   | Copiar texto de la consola                                     |
| `Ctrl+A`                  | Seleccionar todo el contenido                                  |

> **💡 Nota:** El autocompletado usa `rlcompleter` y reconoce todo el namespace inyectado (`app`, `graph`, `selected_node`) más cualquier variable que definas en la sesión.

---

## 7. Recetas avanzadas

### Cambiar un parámetro interno de un nodo

Los parámetros de los nodos no son atributos planos. Están anidados dentro del diccionario `params`, que a su vez tiene subsecciones como `'preset'`, `'formula'` o `'advanced'`. Nunca hagas `nodo.amplitude = 3.0`; eso crea un atributo nuevo en el objeto pero no modifica el parámetro real.

#### Caso A: modificar un preset (seno, cuadrada, etc.)

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Caso B: usar una fórmula custom

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

La expresión usa `t` como variable de tiempo. Los valores en `vars` son los símbolos que podés referenciar en la fórmula. Si omitís `vars`, el nodo usará valores por defecto y la fórmula puede no reflejar el cambio.

#### Caso C: cambiar parámetros avanzados (sample rate, duración)

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Caso D: cambiar parámetro de un nodo no generador (ej. FFT)

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 Nota:** `getattr(obj, '_generate_signal', lambda: None)()` es un patrón seguro: si el método existe (nodos generadores), lo llama; si no, no hace nada y no lanza error. Para nodos procesadores, solo `graph.execute_flow()` es suficiente.

### Listar todas las categorías de nodos disponibles

El import carga el catálogo, pero no lo muestra automáticamente. Recordá que en Python un import exitoso no imprime nada; debés evaluar el objeto.

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

Para ver un resumen legible:

```python
>>> for cat, nodos in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(nodos)} nodos")
```

#### Alternativa: listar nombres de nodos por categoría

```python
>>> {cat: [n.__name__ for n in nodos] for cat, nodos in NODE_CATEGORIES.items()}
```

### Ver la ayuda de cualquier método

```python
>>> help(graph.connect_nodes)
```

Aparece la docstring directamente en la consola. Es útil para descubrir qué argumentos espera un método sin abrir el código fuente.

#### Alternativa: ver atributos filtrados

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. ¿Qué hacer si algo sale mal?

- **Error en rojo en la consola:** el traceback se muestra completo. La aplicación no se cierra; podés corregir el comando y volver a intentar.
- **La interfaz se congela:** probablemente escribiste un bucle infinito. La consola ejecuta en un hilo separado, pero si el bucle afecta el GUI thread, reiniciá la aplicación.
- **`None` inesperado:** si un nodo devuelve `None` en lugar de datos, verificá que esté conectado aguas arriba (`graph.connections`) y que el flujo se haya ejecutado (`graph.execute_flow()`).
- **`ValueError: too many values to unpack`:** estás intentando desempaquetar un dict como si fuera una tupla. Usá `result.keys()` primero.
- **`ValueError: not enough values to unpack`:** esperás 2 valores pero el nodo devuelve 1 (dict) o 3 (espectrograma). Inspeccioná con `type(result)` antes de desempaquetar.
- **`AttributeError`:** el objeto no tiene ese atributo. Usá `dir(obj)` o `[a for a in dir(obj) if 'palabra' in a.lower()]` para descubrir el nombre correcto.
- **No pasa nada al ejecutar:** revisá que haya al menos un nodo fuente conectado a la cadena y que `graph.execute_flow()` se haya llamado. Los nodos procesadores no generan datos solos.
- **El plot no cambia:** asegurate de llamar `graph.execute_flow()` después de modificar parámetros. Solo cambiar `params` no recalcula automáticamente.

---

© 2026 FloWorks — Laboratorio de Señales
