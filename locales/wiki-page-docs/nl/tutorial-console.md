# 🧪 Interactieve Console-tutorial van FloWorks

Welkom bij het experimentele laboratorium van FloWorks. Deze sectie is voor meer gevorderde gebruikers; het is een **Python**-terminal om alles te besturen dat te maken heeft met het canvas, namelijk knopen en hun verbindingen, sequentieel en via regels code. Het is een terminal gekoppeld aan het programma dat het kan besturen en gedragingen of routines kan bepalen voor veeleisendere gebruikers.

Deze gids laat je **stap voor stap** zien hoe je stroomdiagrammen kunt besturen en analyseren zonder de muis aan te raken. Elk voorbeeld is gevalideerd in de interactieve console en weerspiegelt de werkelijke datastructuur van het programma.

---

## 1. Het terrein leren kennen

De console injecteert drie globale objecten: `app` (hoofdvenster), `graph` (scène/diagram) en `selected_node` (het knooppunt dat momenteel is geselecteerd op het canvas). Alle commando's vertrekken van deze drie.

### Alle knopen bekijken

```python
>>> graph.nodes
```

**Voorbeelduitvoer:**
```
Knopen in de scène:
  [0] Geavanceerde Signaalgenerator (type: SignalSourceNode, categorie: Sources)
  [1] FFT (type: FFTNode, categorie: Processing)
```

De index tussen haakjes (`[0]`, `[1]`) is je belangrijkste manier om een knoop te benaderen. De volgorde is die van de creatie op het canvas.

#### Alternatief: knopen tellen of filteren op type

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### Alle verbindingen bekijken

```python
>>> graph.connections
```

**Voorbeelduitvoer:**
```
Verbindingen in de scène:
  [0] Geavanceerde Signaalgenerator (out) → FFT (input)
```

De uitvoer toont de naam van het bronknooppunt, de uitgangspoort, de pijl, het doelknooppunt en de ingangspoort. Als een verbinding niet verschijnt, kan de stroom niet worden uitgevoerd.

#### Alternatief: verbindingen van één knoop bekijken

```python
>>> selected_node.connectors
```

### Geselecteerde knoop bekijken

Klik op een knoop op het canvas en voer vervolgens uit:

```python
>>> selected_node
```

**Voorbeelduitvoer:**
```
Knoop: FFT
  Type: FFTNode
  Categorie: Processing
  Poorten: ['input', 'output', 'magnitude', 'phase']
```

> **💡 Nota:** Als er geen knoop is geselecteerd, heeft `selected_node` de waarde `None`. Het selecteren van een knoop werkt ook automatisch de zijdelingse parametertabel bij.

#### Alternatief: een knoop selecteren via code

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. Knopen en verbindingen manipuleren zonder muis

### Een nieuwe knoop maken

Je moet de exacte klassenaam van de knoop kennen (zoals in de catalogus). De argumenten zijn: `(type, x, y)`.

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

De knoop verschijnt op het canvas op coördinaten (300, 200). Als je de exacte naam niet kent, som dan de categorieën op (zie sectie 7).

#### Alternatief: meerdere knopen tegelijk maken

```python
>>> for i, typ in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(typ, 100 + i*200, 300)
```

### Knopen handmatig verbinden

Syntax: `graph.connect_nodes(bron, bestemming, 'uitgangspoort', 'ingangspoort')`. De poorten hangen af van elke knoop; neem nooit de namen aan.

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 Nota:** Controleer altijd `graph.nodes[N].PORTS` voordat je verbindt. Een FFT-knoop heeft `'input'` en `'magnitude'`; een generator heeft `'output'`.

#### Alternatief: verbinden met de standaardpoort

Als je de exacte naam van de ingangspoort niet kent, accepteren sommige knopen `None` om de eerste beschikbare te gebruiken:

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### Een knoop verwijderen

```python
>>> graph.remove_node(graph.nodes[2])
```

Verwijdert de knoop en alle bijbehorende verbindingen. De indexen in `graph.nodes` worden opnieuw gerangschikt, dus bewaar geen oude referenties.

#### Alternatief: alle knopen van een categorie verwijderen

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### Poorten van een knoop bekijken

```python
>>> graph.nodes[1].PORTS
```

**Voorbeelduitvoer:**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

De sleutel is de naam van de poort (string). De waarde is een tuple met de visuele positie. Alleen de sleutels interesseren je voor het verbinden.

#### Alternatief: poorten als eenvoudige lijst bekijken

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. Stroom uitvoeren en resultaten bekijken

### De hele graaf uitvoeren

```python
>>> graph.execute_flow()
```

Deze methode behoort tot het diagram (`graph`), niet tot het hoofdvenster. Het doorloopt alle knopen in topologische volgorde, voert elk uit en slaat de resultaten in de cache op. Het retourneert niets; de gegevens blijven intern opgeslagen.

#### Alternatief: berekening van een specifieke tak forceren

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

Dit herberekent de hele boom stroomopwaarts van het aangeduide knooppunt en retourneert het resultaat rechtstreeks, zonder de globale cache te wijzigen.

### Gecachte gegevens van een knoop bekijken

Als je toegang nodig hebt tot de verwerkte gegevens van een specifieke knoop, kun je dit op twee manieren doen:

#### Directe manier (per object)
```python
>>> graph.node_values[graph.nodes[1]]
```

**Voorbeelduitvoer:**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ Waarschuwing:** Deze vorm kan mislukken met `KeyError` als de volgorde van de knopen in de scène is gewijzigd (bijv. bij het verwijderen of toevoegen van knopen) of als de objectinstantie niet exact overeenkomt met de sleutel die in het woordenboek is opgeslagen.

#### Alternatieve manier (per positie)
```python
>>> list(graph.node_values.values())[1]
```

Deze vorm is **stabieler** omdat deze niet afhankelijk is van de exacte identiteit van het object. De volgorde van de waarden volgt de volgorde waarin de knopen zijn uitgevoerd tijdens de laatste `graph.execute_flow()`. De index `[1]` komt overeen met de tweede knoop in die volgorde.

> **💡 Nota:** Als je de index van elke knoop in de uitvoervolgorde wilt zien, kun je het volgende gebruiken:
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ Let op:** `graph.node_values` retourneert niet altijd direct een array. Voor verwerkingsknopen (FFT, filters, etc.) retourneert het een **woordenboek** waarbij elke sleutel een uitgangspoort is. Voor bronknopen retourneert het een tuple `(x, y)`.

#### Alternatief: gegevens van alle knopen in één regel bekijken
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### Toegang tot de Y-as van een bronknoop

Bronknopen (SignalSourceNode, FileInputNode, etc.) retourneren bij uitvoering een tuple `(tijd, signaal)`. Om alleen de Y-as te verkrijgen:

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**Voorbeelduitvoer:**
```
Scalar NumPy (float64): 1.0
```

#### Alternatief: de X-as (tijd) verkrijgen

```python
>>> x = result[0]
>>> x[:5]
```

### Toewijzingen in Python: een vitaal detail

In Python zijn toewijzingen (`=`) **instructies**, geen expressies. De console drukt niets af na `x, y = ...` omdat er geen retourwaarde is.

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

Om te verifiëren dat het werkte, evalueer je de variabele in de volgende regel:

```python
>>> x
>>> y.shape
```

Of gebruik `;` om een expressie op dezelfde regel te ketenen:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

Of gebruik expliciet `print()`:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. Tekenen op het hoofdgrafiek

### Grafiek wissen

```python
>>> app.plot_widget.clear_plot()
```

#### Alternatief: wissen en onmiddellijk opnieuw tekenen

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### Een willekeurig signaal uit de console tekenen

Je kunt arrays maken met NumPy en ze rechtstreeks naar het plotwidget sturen, zonder door een knoop te gaan.

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### Alternatief: een som van sinusen tekenen

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### Resultaat van een bronknoop tekenen

Omdat de bronknoop `(x, y)` retourneert, kun je het direct uitpakken:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### Resultaat van een FFT-knoop tekenen (meerdere poorten)

Knopen met meerdere uitgangen (FFT, tijd-frequentie-analyse, etc.) retourneren geen eenvoudige tuple. Ze retourneren een `dict`, waarbij elke sleutel een uitgangspoort is.

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**Voorbeelduitvoer:**
```
dict_keys(['output', 'magnitude', 'phase'])
```

Merk op dat `'output'` `None` kan zijn als de knoop geen generieke poort heeft. De nuttige uitgangen zijn `'magnitude'` en `'phase'`, die op hun beurt tuples `(frequenties, waarden)` zijn:

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### Alternatief: fase tekenen in plaats van magnitude

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### Alternatief: twee signalen overlappen

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # andere knoop
>>> app.plot_widget.plot_waveform(x2, y2)  # overlapt
```

> **💡 Nota:** Als je probeert `x, y = result` te doen met een dict, zal Python `ValueError: too many values to unpack` opwerpen. Controleer altijd eerst met `type(result)` en `result.keys()`.

---

## 5. De toepassing tijdens uitvoering wijzigen

### Zijdelingse parametertabel bijwerken

Als je een parameter via code wijzigt en wilt dat de zijdelingse tabel de wijziging weerspiegelt:

```python
>>> app.workspace_table.populate()
```

Deze methode accepteert geen argumenten. Het vernieuwt de tabel met de huidige waarden van de geselecteerde knoop.

#### Alternatief: selectie van een andere knoop forceren en vernieuwen

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### Een knoop toevoegen vanuit de werkbalk

```python
>>> app.add_node('SignalSourceNode')
```

Het is equivalent aan het indrukken van de "+"-knop op de werkbalk. De knoop wordt op een vooraf bepaalde positie op het canvas geplaatst.

### Venstertitel wijzigen

`setWindowTitle` is de native Qt-methode. Het werkt, maar houd er rekening mee dat de toepassing een timer of gebeurtenis kan hebben die `update_title()` aanroept en het automatisch overschrijft.

```python
>>> app.setWindowTitle('Mijn signaallaboratorium')
>>> app.windowTitle()
```

Om de "officiële" titel te herstellen die de app berekent vanuit haar interne staat (projectnaam, bestand, etc.):

```python
>>> app.update_title()
```

#### Alternatief: titel met projectnaam

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. Navigatie en productiviteit in de console

De console is niet zomaar een `print()`. Het heeft geschiedenis, autocompletie en multiline-blokken.

| Toets / Commando           | Actie                                                         |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | Navigeren door uitgevoerde commandogeschiedenis                |
| `Tab`                     | Autocompletie van variabelen, attributen en methoden van namespace     |
| `Ctrl + L`                | Hele console wissen (wist tekst, niet de Python-toestand)  |
| `if`, `for`, `def`, `class` | Prompt verandert van `>>>` naar `...` voor multiline-blokken     |
| `Ctrl+C` (in selectie)   | Tekst uit console kopiëren                                     |
| `Ctrl+A`                  | Alle inhoud selecteren                                  |

> **💡 Nota:** Autocompletie gebruikt `rlcompleter` en herkent de hele geïnjecteerde namespace (`app`, `graph`, `selected_node`) plus elke variabele die je in de sessie definieert.

---

## 7. Geavanceerde recepten

### Een interne parameter van een knoop wijzigen

Knoopparameters zijn geen platte attributen. Ze zijn genest in het `params`-woordenboek, dat op zijn beurt subsecties heeft zoals `'preset'`, `'formula'` of `'advanced'`. Doe nooit `knoop.amplitude = 3.0`; dat creëert een nieuw attribuut op het object, maar wijzigt niet de echte parameter.

#### Geval A: een preset wijzigen (sinus, vierkant, etc.)

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Geval B: een aangepaste formule gebruiken

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

De expressie gebruikt `t` als tijdsvariabele. De waarden in `vars` zijn de symbolen waarnaar je in de formule kunt verwijzen. Als je `vars` weglaat, zal de knoop standaardwaarden gebruiken en de formule zou de wijziging mogelijk niet weerspiegelen.

#### Geval C: geavanceerde parameters wijzigen (sample rate, duur)

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Geval D: parameter van een niet-generator knoop wijzigen (bijv. FFT)

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 Nota:** `getattr(obj, '_generate_signal', lambda: None)()` is een veilig patroon: als de methode bestaat (generatorknopen), roept het deze aan; zo niet, dan doet het niets en werpt geen fout. Voor verwerkingsknopen is alleen `graph.execute_flow()` voldoende.

### Alle beschikbare knooppuntcategorieën oplijsten

De import laadt de catalogus, maar toont deze niet automatisch. Onthoud dat in Python een succesvolle import niets afdrukt; je moet het object evalueren.

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

Voor een leesbare samenvatting:

```python
>>> for cat, knopen in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(knopen)} knopen")
```

#### Alternatief: knooppuntnamen per categorie oplijsten

```python
>>> {cat: [n.__name__ for n in knopen] for cat, knopen in NODE_CATEGORIES.items()}
```

### Help van elke methode bekijken

```python
>>> help(graph.connect_nodes)
```

De docstring verschijnt rechtstreeks in de console. Het is handig om te ontdekken welke argumenten een methode verwacht zonder de broncode te openen.

#### Alternatief: gefilterde attributen bekijken

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. Wat te doen als er iets misgaat?

- **Fout in rood in de console:** de traceback wordt volledig weergegeven. De toepassing sluit niet; je kunt het commando corrigeren en opnieuw proberen.
- **De interface bevriest:** je hebt waarschijnlijk een oneindige lus geschreven. De console draait in een aparte thread, maar als de lus de GUI-thread beïnvloedt, herstart de toepassing.
- **Onverwachte `None`:** als een knoop `None` retourneert in plaats van gegevens, verifieer dan dat deze stroomopwaarts is verbonden (`graph.connections`) en dat de stroom is uitgevoerd (`graph.execute_flow()`).
- **`ValueError: too many values to unpack`:** je probeert een dict uit te pakken alsof het een tuple is. Gebruik eerst `result.keys()`.
- **`ValueError: not enough values to unpack`:** je verwacht 2 waarden maar de knoop retourneert 1 (dict) of 3 (spectrogram). Inspecteer met `type(result)` voordat je uitpakt.
- **`AttributeError`:** het object heeft dat attribuut niet. Gebruik `dir(obj)` of `[a for a in dir(obj) if 'woord' in a.lower()]` om de juiste naam te ontdekken.
- **Er gebeurt niets bij uitvoering:** controleer dat er minstens één bronknoop aan de keten is verbonden en dat `graph.execute_flow()` is aangeroepen. Verwerkingsknopen genereren geen gegevens op zichzelf.
- **Het plot verandert niet:** zorg ervoor dat je `graph.execute_flow()` aanroept na het wijzigen van parameters. Alleen het wijzigen van `params` herberekent niet automatisch.

---

© 2026 FloWorks — Signaallaboratorium
