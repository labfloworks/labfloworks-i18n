# 🧪 Tutorial Interactiv al Consolei FloWorks

Bine ați venit în laboratorul experimental FloWorks. Această secțiune este pentru utilizatori mai avansați; este un terminal **Python** pentru controlul a tot ceea ce este asociat cu pânza, adică nodurile și conexiunile lor, secvențial și prin linii de cod. Este un terminal legat de program care îl poate controla și determina comportamente sau rutine pentru utilizatorii mai exigenți.

Acest ghid vă va arăta **pas cu pas** cum să controlați și să analizați diagramele de flux fără să atingeți mouse-ul. Fiecare exemplu a fost validat în consola interactivă și reflectă structura reală de date a programului.

---

## 1. Să cunoaștem terenul

Consola injectează trei obiecte globale: `app` (fereastra principală), `graph` (scena/diagrama) și `selected_node` (nodul selectat în prezent pe pânză). Toate comenzile pornesc de la aceste trei.

### A vedea toate nodurile

```python
>>> graph.nodes
```

**Exemplu de ieșire:**
```
Noduri în scenă:
  [0] Generator Avansat de Semnale (tip: SignalSourceNode, categorie: Sources)
  [1] FFT (tip: FFTNode, categorie: Processing)
```

Indexul dintre paranteze drepte (`[0]`, `[1]`) este principala modalitate de a accesa un nod. Ordinea este cea a creării pe pânză.

#### Alternativă: a număra nodurile sau a filtra după tip

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### A vedea toate conexiunile

```python
>>> graph.connections
```

**Exemplu de ieșire:**
```
Conexiuni în scenă:
  [0] Generator Avansat de Semnale (out) → FFT (input)
```

Ieșirea arată numele nodului sursă, portul de ieșire, săgeata, nodul destinație și portul de intrare. Dacă o conexiune nu apare, fluxul nu va putea fi executat.

#### Alternativă: a vedea conexiunile unui singur nod

```python
>>> selected_node.connectors
```

### A vedea nodul selectat

Faceți clic pe un nod de pe pânză și apoi executați:

```python
>>> selected_node
```

**Exemplu de ieșire:**
```
Nod: FFT
  Tip: FFTNode
  Categorie: Processing
  Porturi: ['input', 'output', 'magnitude', 'phase']
```

> **💡 Notă:** Dacă nu este selectat niciun nod, `selected_node` are valoarea `None`. Selectarea unui nod actualizează automat și tabelul lateral de parametri.

#### Alternativă: a selecta un nod prin cod

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. A manipula noduri și conexiuni fără mouse

### A crea un nod nou

Trebuie să cunoașteți numele exact al clasei nodului (la fel ca în catalog). Argumentele sunt: `(tip, x, y)`.

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

Nodul apare pe pânză la coordonatele (300, 200). Dacă nu cunoașteți numele exact, listați categoriile (vezi secțiunea 7).

#### Alternativă: a crea mai multe noduri simultan

```python
>>> for i, tip in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(tip, 100 + i*200, 300)
```

### A conecta noduri manual

Sintaxa: `graph.connect_nodes(sursa, destinatie, 'port_iesire', 'port_intrare')`. Porturile depind de fiecare nod; nu presupuneți niciodată numele lor.

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 Notă:** Verificați întotdeauna `graph.nodes[N].PORTS` înainte de a conecta. Un nod FFT are `'input'` și `'magnitude'`; un generator are `'output'`.

#### Alternativă: a conecta la portul implicit

Dacă nu știți numele exact al portului de intrare, unele noduri acceptă `None` pentru a folosi primul disponibil:

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### A elimina un nod

```python
>>> graph.remove_node(graph.nodes[2])
```

Elimină nodul și toate conexiunile asociate. Indicii din `graph.nodes` se reordonează, așa că nu păstrați referințe vechi.

#### Alternativă: a elimina toate nodurile dintr-o categorie

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### A vedea porturile unui nod

```python
>>> graph.nodes[1].PORTS
```

**Exemplu de ieșire:**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

Cheia este numele portului (string). Valoarea este o tuplă cu poziția vizuală. Doar cheile vă interesează pentru conectare.

#### Alternativă: a vedea porturile ca listă simplă

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. A executa fluxul și a vedea rezultatele

### A executa întregul graf

```python
>>> graph.execute_flow()
```

Această metodă aparține diagramei (`graph`), nu ferestrei principale. Parcurge toate nodurile în ordine topologică, execută fiecare și cachează rezultatele. Nu returnează nimic; datele rămân stocate intern.

#### Alternativă: a forța calculul unei ramuri specifice

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

Aceasta recalculează tot arborele în amonte al nodului indicat și returnează rezultatul direct, fără a modifica cache-ul global.

### A vedea datele cache-uite ale unui nod

Dacă aveți nevoie să accesați datele procesate ale unui nod specific, puteți face acest lucru în două moduri:

#### Mod direct (după obiect)
```python
>>> graph.node_values[graph.nodes[1]]
```

**Exemplu de ieșire:**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ Avertisment:** Această formă poate eșua cu `KeyError` dacă ordinea nodurilor în scenă s-a schimbat (de exemplu, la eliminarea sau adăugarea de noduri) sau dacă instana obiectului nu se potrivește exact cu cheia salvată în dicționar.

#### Mod alternativ (după poziție)
```python
>>> list(graph.node_values.values())[1]
```

Această formă este **mai stabilă** deoarece nu depinde de identitatea exactă a obiectului. Ordinea valorilor urmează secvența în care nodurile au fost executate în timpul ultimului `graph.execute_flow()`. Indexul `[1]` corespunde celui de-al doilea nod în acea secvență.

> **💡 Notă:** Dacă doriți să vedeți indexul fiecărui nod în ordinea de execuție, puteți folosi:
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ Atenție:** `graph.node_values` nu returnează întotdeauna un array direct. Pentru noduri procesatoare (FFT, filtre etc.) returnează un **dicționar** unde fiecare cheie este un port de ieșire. Pentru noduri sursă, returnează o tuplă `(x, y)`.

#### Alternativă: a vedea datele tuturor nodurilor într-o singură linie
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### Accesul la axa Y a unui nod sursă

Nodurile generatoare (SignalSourceNode, FileInputNode etc.) returnează o tuplă `(timp, semnal)` la execuție. Pentru a obține doar axa Y:

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**Exemplu de ieșire:**
```
Scalar NumPy (float64): 1.0
```

#### Alternativă: a obține axa X (timp)

```python
>>> x = result[0]
>>> x[:5]
```

### Atribuiri în Python: un detaliu vital

În Python, atribuirile (`=`) sunt **instrucțiuni**, nu expresii. Consola nu tipărește nimic după `x, y = ...` deoarece nu există o valoare de returnare.

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

Pentru a verifica că a funcționat, evaluați variabila în linia următoare:

```python
>>> x
>>> y.shape
```

Sau folosiți `;` pentru a înlănțui o expresie pe aceeași linie:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

Sau folosiți `print()` explicit:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. A desena pe graficul principal

### A curăța graficul

```python
>>> app.plot_widget.clear_plot()
```

#### Alternativă: a curăța și a redesena imediat

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### A desena un semnal arbitrar din consolă

Puteți crea array-uri cu NumPy și le puteți trimite direct la widget-ul de plotare, fără a trece prin niciun nod.

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### Alternativă: a desena o sumă de sinusuri

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### A desena rezultatul unui nod sursă

Deoarece nodul sursă returnează `(x, y)`, îl puteți despacheta direct:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### A desena rezultatul unui nod FFT (porturi multiple)

Nodurile cu ieșiri multiple (FFT, analiză timp-frecvență etc.) nu returnează o tuplă simplă. Returnează un `dict`, unde fiecare cheie este un port de ieșire.

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**Exemplu de ieșire:**
```
dict_keys(['output', 'magnitude', 'phase'])
```

Observați că `'output'` poate fi `None` dacă nodul nu are un port generic. Ieșirile utile sunt `'magnitude'` și `'phase'`, care la rândul lor sunt tupluri `(frecvențe, valori)`:

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### Alternativă: a desena faza în loc de magnitudine

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### Alternativă: a suprapune două semnale

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # alt nod
>>> app.plot_widget.plot_waveform(x2, y2)  # se suprapune
```

> **💡 Notă:** Dacă încercați să faceți `x, y = result` cu un dict, Python va lansa `ValueError: too many values to unpack`. Verificați întotdeauna cu `type(result)` și `result.keys()` înainte de despachetare.

---

## 5. A modifica aplicația din mers

### A actualiza tabelul lateral de parametri

Dacă modificați un parametru prin cod și doriți ca tabelul lateral să reflecte schimbarea:

```python
>>> app.workspace_table.populate()
```

Această metodă nu primește argumente. Reîmprospătează tabelul cu valorile actuale ale nodului selectat.

#### Alternativă: a forța selectarea altui nod și reîmprospătarea

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### A adăuga un nod din bara de instrumente

```python
>>> app.add_node('SignalSourceNode')
```

Este echivalent cu apăsarea butonului "+" din bara de instrumente. Nodul este plasat într-o poziție prestabilită pe pânză.

### A schimba titlul ferestrei

`setWindowTitle` este metoda nativă Qt. Funcționează, dar rețineți că aplicația poate avea un timer sau eveniment care apelează `update_title()` și îl suprascrie automat.

```python
>>> app.setWindowTitle('Laboratorul meu de semnale')
>>> app.windowTitle()
```

Pentru a restaura titlul "oficial" pe care aplicația îl calculează din starea sa internă (nume proiect, fișier etc.):

```python
>>> app.update_title()
```

#### Alternativă: titlu cu nume de proiect

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. Navigare și productivitate în consolă

Consola nu este doar un `print()`. Are istoric, autocompletare și blocuri multilinie.

| Tastă / Comandă           | Acțiune                                                         |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | A naviga prin istoricul comenzilor executate                |
| `Tab`                     | Autocompletare variabile, atribute și metode ale namespace-ului     |
| `Ctrl + L`                | A curăța întreaga consolă (șterge textul, nu starea Python)  |
| `if`, `for`, `def`, `class` | Promptul se schimbă din `>>>` în `...` pentru blocuri multilinie     |
| `Ctrl+C` (în selecție)   | A copia text din consolă                                     |
| `Ctrl+A`                  | A selecta tot conținutul                                  |

> **💡 Notă:** Autocompletarea folosește `rlcompleter` și recunoaște tot namespace-ul injectat (`app`, `graph`, `selected_node`) plus orice variabilă definiți în sesiune.

---

## 7. Rețete avansate

### A schimba un parametru intern al unui nod

Parametrii nodurilor nu sunt atribute plate. Sunt imbricați în dicționarul `params`, care la rândul său are subsecțiuni precum `'preset'`, `'formula'` sau `'advanced'`. Nu faceți niciodată `nod.amplitude = 3.0`; asta creează un atribut nou pe obiect, dar nu modifică parametrul real.

#### Cazul A: a modifica un preset (sin, pătrat etc.)

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Cazul B: a folosi o formulă personalizată

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

Expresia folosește `t` ca variabilă de timp. Valorile din `vars` sunt simbolurile la care puteți face referire în formulă. Dacă omiteți `vars`, nodul va folosi valori implicite și formula s-ar putea să nu reflecte schimbarea.

#### Cazul C: a schimba parametri avansați (rată de eșantionare, durată)

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Cazul D: a schimba parametrul unui nod negenerator (ex. FFT)

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 Notă:** `getattr(obj, '_generate_signal', lambda: None)()` este un pattern sigur: dacă metoda există (noduri generatoare), o apelează; dacă nu, nu face nimic și nu lansează eroare. Pentru noduri procesatoare, doar `graph.execute_flow()` este suficient.

### A lista toate categoriile de noduri disponibile

Importul încarcă catalogul, dar nu îl afișează automat. Rețineți că în Python un import reușit nu tipărește nimic; trebuie să evaluați obiectul.

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

Pentru un rezumat lizibil:

```python
>>> for cat, noduri in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(noduri)} noduri")
```

#### Alternativă: a lista numele nodurilor pe categorie

```python
>>> {cat: [n.__name__ for n in noduri] for cat, noduri in NODE_CATEGORIES.items()}
```

### A vedea ajutorul pentru orice metodă

```python
>>> help(graph.connect_nodes)
```

Docstring-ul apare direct în consolă. Este util pentru a descoperi ce argumente așteaptă o metodă fără a deschide codul sursă.

#### Alternativă: a vedea atribute filtrate

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. Ce să faci dacă ceva nu merge?

- **Eroare în roșu în consolă:** traceback-ul se afișează complet. Aplicația nu se închide; puteți corecta comanda și reîncerca.
- **Interfața se îngheață:** probabil ați scris o buclă infinită. Consola rulează într-un fir separat, dar dacă bucla afectează firul GUI, reporniți aplicația.
- **`None` neașteptat:** dacă un nod returnează `None` în loc de date, verificați că este conectat în amonte (`graph.connections`) și că fluxul a fost executat (`graph.execute_flow()`).
- **`ValueError: too many values to unpack`:** încercați să despachetați un dict ca și cum ar fi o tuplă. Folosiți mai întâi `result.keys()`.
- **`ValueError: not enough values to unpack`:** așteptați 2 valori, dar nodul returnează 1 (dict) sau 3 (spectrogramă). Inspectați cu `type(result)` înainte de despachetare.
- **`AttributeError`:** obiectul nu are acel atribut. Folosiți `dir(obj)` sau `[a for a in dir(obj) if 'cuvant' in a.lower()]` pentru a descoperi numele corect.
- **Nu se întâmplă nimic la executare:** verificați că există cel puțin un nod sursă conectat la lanț și că `graph.execute_flow()` a fost apelat. Nodurile procesatoare nu generează date singure.
- **Graficul nu se schimbă:** asigurați-vă că apelați `graph.execute_flow()` după modificarea parametrilor. Doar schimbarea `params` nu recalculează automat.

---

© 2026 FloWorks — Laborator de Semnale
