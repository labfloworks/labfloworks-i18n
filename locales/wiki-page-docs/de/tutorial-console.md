# 🧪 Interaktives Tutorial der FloWorks-Konsole

Willkommen im Experimentierlabor von FloWorks. Dies ist ein Abschnitt für fortgeschrittenere Benutzer: Es handelt sich um ein **Python**-Terminal zur Steuerung von allem, was mit der Zeichenfläche verbunden ist, also Knoten und deren Verbindungen, sequenziell und durch Codezeilen; es ist ein mit dem Programm verknüpftes Terminal, das es steuern und Verhaltensweisen oder Routinen für die Bequemlichkeit anspruchsvollerer Benutzer bestimmen kann.

Diese Anleitung zeigt dir **Schritt für Schritt**, wie du deine Flussdiagramme steuerst und analysierst, ohne die Maus zu berühren. Jedes Beispiel wurde in der interaktiven Konsole validiert und spiegelt die tatsächliche Datenstruktur des Programms wider.

---

## 1. Das Gelände kennenlernen

Die Konsole injiziert drei globale Objekte: `app` (Hauptfenster), `graph` (Szene/Diagramm) und `selected_node` (derzeit auf der Zeichenfläche ausgewählter Knoten). Alle Befehle gehen von diesen dreien aus.

### Alle Knoten anzeigen

```python
>>> graph.nodes
```

**Beispielausgabe:**
```
Knoten in der Szene:
  [0] Erweiterter Signalgenerator (Typ: SignalSourceNode, Kategorie: Sources)
  [1] FFT (Typ: FFTNode, Kategorie: Processing)
```

Der Index in eckigen Klammern (`[0]`, `[1]`) ist deine Hauptmethode, um auf einen Knoten zuzugreifen. Die Reihenfolge entspricht der Erstellung auf der Zeichenfläche.

#### Alternative: Knoten zählen oder nach Typ filtern

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### Alle Verbindungen anzeigen

```python
>>> graph.connections
```

**Beispielausgabe:**
```
Verbindungen in der Szene:
  [0] Erweiterter Signalgenerator (out) → FFT (input)
```

Die Ausgabe zeigt den Namen des Quellknotens, den Ausgangsport, den Pfeil, den Zielknoten und den Eingangsport. Wenn eine Verbindung nicht erscheint, kann der Fluss nicht ausgeführt werden.

#### Alternative: Verbindungen eines einzelnen Knotens anzeigen

```python
>>> selected_node.connectors
```

### Ausgewählten Knoten anzeigen

Klicke auf einen Knoten auf der Zeichenfläche und führe dann aus:

```python
>>> selected_node
```

**Beispielausgabe:**
```
Knoten: FFT
  Typ: FFTNode
  Kategorie: Processing
  Ports: ['input', 'output', 'magnitude', 'phase']
```

> **💡 Hinweis:** Wenn kein Knoten ausgewählt ist, ist `selected_node` `None`. Die Auswahl eines Knotens aktualisiert auch automatisch die seitliche Parametertabelle.

#### Alternative: Knoten per Code auswählen

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. Knoten und Verbindungen ohne Maus manipulieren

### Neuen Knoten erstellen

Du musst den genauen Klassennamen des Knotens kennen (genau wie im Katalog). Die Argumente sind: `(Typ, x, y)`.

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

Der Knoten erscheint auf der Zeichenfläche bei den Koordinaten (300, 200). Wenn du den genauen Namen nicht kennst, liste die Kategorien auf (siehe Abschnitt 7).

#### Alternative: Mehrere Knoten auf einmal erstellen

```python
>>> for i, tipo in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(tipo, 100 + i*200, 300)
```

### Knoten manuell verbinden

Syntax: `graph.connect_nodes(Quelle, Ziel, 'Ausgangsport', 'Eingangsport')`. Die Ports hängen von jedem Knoten ab; nimm die Namen niemals an.

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 Hinweis:** Überprüfe immer `graph.nodes[N].PORTS`, bevor du verbindest. Ein FFT-Knoten hat `'input'` und `'magnitude'`; ein Generator hat `'output'`.

#### Alternative: Mit dem Standardport verbinden

Wenn du den genauen Namen des Eingangsports nicht kennst, akzeptieren einige Knoten `None`, um den ersten verfügbaren zu verwenden:

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### Knoten löschen

```python
>>> graph.remove_node(graph.nodes[2])
```

Löscht den Knoten und alle zugehörigen Verbindungen. Die Indizes von `graph.nodes` werden neu angeordnet, also bewahre keine alten Referenzen auf.

#### Alternative: Alle Knoten einer Kategorie löschen

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### Ports eines Knotens anzeigen

```python
>>> graph.nodes[1].PORTS
```

**Beispielausgabe:**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

Der Schlüssel ist der Portname (String). Der Wert ist ein Tupel mit der visuellen Position. Für das Verbinden sind nur die Schlüssel von Interesse.

#### Alternative: Ports als einfache Liste anzeigen

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. Fluss ausführen und Ergebnisse anzeigen

### Gesamten Graphen ausführen

```python
>>> graph.execute_flow()
```

Diese Methode gehört zum Diagramm (`graph`), nicht zum Hauptfenster. Sie durchläuft alle Knoten in topologischer Reihenfolge, führt jeden aus und cached die Ergebnisse. Sie gibt nichts zurück; die Daten werden intern gespeichert.

#### Alternative: Berechnung eines bestimmten Zweigs erzwingen

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

Dies berechnet den gesamten Baum oberhalb des angegebenen Knotens neu und gibt das Ergebnis direkt zurück, ohne den globalen Cache zu ändern.

### Gecachte Daten eines Knotens anzeigen

Wenn du auf die verarbeiteten Daten eines bestimmten Knotens zugreifen musst, kannst du dies auf zwei Arten tun:

#### Direkte Methode (nach Objekt)
```python
>>> graph.node_values[graph.nodes[1]]
```

**Beispielausgabe:**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ Warnung:** Diese Form kann mit `KeyError` fehlschlagen, wenn sich die Reihenfolge der Knoten in der Szene geändert hat (z. B. beim Löschen oder Hinzufügen von Knoten) oder wenn die Objektinstanz nicht genau mit dem im Wörterbuch gespeicherten Schlüssel übereinstimmt.

#### Alternative Methode (nach Position)
```python
>>> list(graph.node_values.values())[1]
```

Diese Form ist **stabiler**, weil sie nicht von der genauen Objektidentität abhängt. Die Reihenfolge der Werte folgt der Sequenz, in der die Knoten während des letzten `graph.execute_flow()` ausgeführt wurden. Der Index `[1]` entspricht dem zweiten Knoten in dieser Sequenz.

> **💡 Hinweis:** Wenn du den Index jedes Knotens in der Ausführungsreihenfolge sehen möchtest, kannst du verwenden:
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ Vorsicht:** `graph.node_values` gibt nicht immer direkt ein Array zurück. Für Prozessorknoten (FFT, Filter usw.) gibt es ein **Wörterbuch** zurück, bei dem jeder Schlüssel ein Ausgangsport ist. Für Quellknoten gibt es ein Tupel `(x, y)` zurück.

#### Alternative: Daten aller Knoten in einer Zeile anzeigen
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### Auf die Y-Achse eines Quellknotens zugreifen

Generator-Knoten (SignalSourceNode, FileInputNode usw.) geben bei der Ausführung ein Tupel `(Zeit, Signal)` zurück. Um nur die Y-Achse zu erhalten:

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**Beispielausgabe:**
```
Scalar NumPy (float64): 1.0
```

#### Alternative: X-Achse (Zeit) erhalten

```python
>>> x = result[0]
>>> x[:5]
```

### Zuweisungen in Python: ein wichtiges Detail

In Python sind Zuweisungen (`=`) **Anweisungen**, keine Ausdrücke. Die Konsole druckt nach `x, y = ...` nichts, weil es keinen Rückgabewert gibt.

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

Um zu überprüfen, ob es funktioniert hat, werte die Variable in der nächsten Zeile aus:

```python
>>> x
>>> y.shape
```

Oder verwende `;`, um einen Ausdruck in derselben Zeile zu verketten:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

Oder verwende explizites `print()`:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. Im Hauptdiagramm zeichnen

### Diagramm löschen

```python
>>> app.plot_widget.clear_plot()
```

#### Alternative: Sofort löschen und neu zeichnen

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### Beliebiges Signal von der Konsole zeichnen

Du kannst Arrays mit NumPy erstellen und sie direkt an das Plot-Widget senden, ohne durch einen Knoten zu gehen.

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### Alternative: Summe von Sinuskurven zeichnen

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### Ergebnis eines Quellknotens zeichnen

Da der Quellknoten `(x, y)` zurückgibt, kannst du direkt entpacken:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### Ergebnis eines FFT-Knotens zeichnen (Mehrfachports)

Knoten mit mehreren Ausgängen (FFT, Zeit-Frequenz-Analyse usw.) geben kein einfaches Tupel zurück. Sie geben ein `dict` zurück, bei dem jeder Schlüssel ein Ausgangsport ist.

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**Beispielausgabe:**
```
dict_keys(['output', 'magnitude', 'phase'])
```

Beachte, dass `'output'` `None` sein kann, wenn der Knoten keinen generischen Port hat. Die nützlichen Ausgänge sind `'magnitude'` und `'phase'`, die wiederum Tupel `(Frequenzen, Werte)` sind:

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### Alternative: Phase statt Betrag zeichnen

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### Alternative: Zwei Signale überlagern

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # anderer Knoten
>>> app.plot_widget.plot_waveform(x2, y2)  # überlagert sich
```

> **💡 Hinweis:** Wenn du versuchst, `x, y = result` mit einem dict zu machen, wird Python `ValueError: too many values to unpack` auslösen. Inspiziere immer mit `type(result)` und `result.keys()`, bevor du entpackst.

---

## 5. Anwendung im laufenden Betrieb modifizieren

### Seitliche Parametertabelle aktualisieren

Wenn du einen Parameter per Code modifizierst und möchtest, dass die seitliche Tabelle die Änderung widerspiegelt:

```python
>>> app.workspace_table.populate()
```

Diese Methode erhält keine Argumente. Sie aktualisiert die Tabelle mit den aktuellen Werten des ausgewählten Knotens.

#### Alternative: Auswahl eines anderen Knotens erzwingen und aktualisieren

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### Knoten aus der Werkzeugleiste hinzufügen

```python
>>> app.add_node('SignalSourceNode')
```

Dies ist gleichbedeutend mit dem Drücken der "+"-Schaltfläche in der Werkzeugleiste. Der Knoten wird an einer vordefinierten Position auf der Zeichenfläche platziert.

### Fenstertitel ändern

`setWindowTitle` ist die native Qt-Methode. Sie funktioniert, aber beachte, dass die Anwendung möglicherweise einen Timer oder ein Ereignis hat, das `update_title()` aufruft und ihn automatisch überschreibt.

```python
>>> app.setWindowTitle('Mein Signallabor')
>>> app.windowTitle()
```

Um den "offiziellen" Titel wiederherzustellen, den die App aus ihrem internen Zustand (Projektname, Datei usw.) berechnet:

```python
>>> app.update_title()
```

#### Alternative: Titel mit Projektname

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. Navigation und Produktivität in der Konsole

Die Konsole ist kein einfaches `print()`. Sie hat Historie, Autovervollständigung und mehrzeilige Blöcke.

| Taste / Befehl           | Aktion                                                         |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | Im Verlauf der ausgeführten Befehle navigieren                |
| `Tab`                     | Variablen, Attribute und Methoden des Namespace vervollständigen     |
| `Ctrl + L`                | Gesamte Konsole löschen (löscht Text, nicht den Python-Zustand)  |
| `if`, `for`, `def`, `class` | Prompt ändert sich von `>>>` zu `...` für mehrzeilige Blöcke     |
| `Ctrl+C` (in Auswahl)   | Text aus der Konsole kopieren                                     |
| `Ctrl+A`                  | Gesamten Inhalt auswählen                                  |

> **💡 Hinweis:** Die Autovervollständigung verwendet `rlcompleter` und erkennt den gesamten injizierten Namespace (`app`, `graph`, `selected_node`) plus alle Variablen, die du in der Sitzung definierst.

---

## 7. Erweiterte Rezepte

### Internen Parameter eines Knotens ändern

Die Parameter der Knoten sind keine flachen Attribute. Sie sind innerhalb des Wörterbuchs `params` verschachtelt, das wiederum Unterabschnitte wie `'preset'`, `'formula'` oder `'advanced'` hat. Mache niemals `node.amplitude = 3.0`; das erstellt ein neues Attribut im Objekt, ändert aber nicht den echten Parameter.

#### Fall A: Ein Preset ändern (Sinus, Rechteck usw.)

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Fall B: Eine benutzerdefinierte Formel verwenden

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

Der Ausdruck verwendet `t` als Zeitvariable. Die Werte in `vars` sind die Symbole, die du in der Formel referenzieren kannst. Wenn du `vars` weglässt, verwendet der Knoten Standardwerte und die Formel spiegelt die Änderung möglicherweise nicht wider.

#### Fall C: Erweiterte Parameter ändern (Abtastrate, Dauer)

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Fall D: Parameter eines Nicht-Generator-Knotens ändern (z. B. FFT)

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 Hinweis:** `getattr(obj, '_generate_signal', lambda: None)()` ist ein sicheres Muster: Wenn die Methode existiert (Generator-Knoten), ruft sie sie auf; wenn nicht, tut sie nichts und löst keinen Fehler aus. Für Prozessorknoten ist nur `graph.execute_flow()` ausreichend.

### Alle verfügbaren Knotenkategorien auflisten

Der Import lädt den Katalog, zeigt ihn aber nicht automatisch an. Denke daran, dass in Python ein erfolgreicher Import nichts ausgibt; du musst das Objekt auswerten.

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

Für eine lesbare Zusammenfassung:

```python
>>> for cat, nodos in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(nodos)} Knoten")
```

#### Alternative: Knotennamen nach Kategorie auflisten

```python
>>> {cat: [n.__name__ for n in nodos] for cat, nodos in NODE_CATEGORIES.items()}
```

### Hilfe für jede Methode anzeigen

```python
>>> help(graph.connect_nodes)
```

Der Docstring erscheint direkt in der Konsole. Dies ist nützlich, um die erwarteten Argumente einer Methode zu entdecken, ohne den Quellcode zu öffnen.

#### Alternative: Gefilterte Attribute anzeigen

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. Was tun, wenn etwas schiefgeht?

- **Fehler in Rot in der Konsole:** Der Traceback wird vollständig angezeigt. Die Anwendung schließt sich nicht; du kannst den Befehl korrigieren und es erneut versuchen.
- **Die Oberfläche friert ein:** Wahrscheinlich hast du eine Endlosschleife geschrieben. Die Konsole läuft in einem separaten Thread, aber wenn die Schleife den GUI-Thread betrifft, starte die Anwendung neu.
- **Unerwartetes `None`:** Wenn ein Knoten `None` statt Daten zurückgibt, überprüfe, ob er stromaufwärts verbunden ist (`graph.connections`) und ob der Fluss ausgeführt wurde (`graph.execute_flow()`).
- **`ValueError: too many values to unpack`:** Du versuchst, ein dict wie ein Tupel zu entpacken. Verwende zuerst `result.keys()`.
- **`ValueError: not enough values to unpack`:** Du erwartest 2 Werte, aber der Knoten gibt 1 (dict) oder 3 (Spektrogramm) zurück. Inspiziere vor dem Entpacken mit `type(result)`.
- **`AttributeError`:** Das Objekt hat dieses Attribut nicht. Verwende `dir(obj)` oder `[a for a in dir(obj) if 'wort' in a.lower()]`, um den richtigen Namen zu finden.
- **Nichts passiert bei Ausführung:** Überprüfe, ob mindestens ein Quellknoten mit der Kette verbunden ist und ob `graph.execute_flow()` aufgerufen wurde. Prozessorknoten erzeugen keine Daten allein.
- **Der Plot ändert sich nicht:** Stelle sicher, dass du `graph.execute_flow()` nach dem Ändern von Parametern aufgerufen hast. Nur das Ändern von `params` berechnet nicht automatisch neu.

---

© 2026 FloWorks — Signallabor
