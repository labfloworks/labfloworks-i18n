# 🧪 Interaktivní konzolový tutoriál FloWorks

Vítejte v experimentálním laboratoři FloWorks. Tato sekce je pro pokročilejší uživatele; jedná se o **Python** terminál pro ovládání všeho souvisejícího s plátnem, tedy uzlů a jejich připojení, sekvenčně a pomocí řádků kódu. Je to terminál propojený s programem, který jej může ovládat a určovat chování nebo rutiny pro náročnější uživatele.

Tato příručka vám **krok za krokem** ukáže, jak ovládat a analyzovat vaše vývojové diagramy bez použití myši. Každý příklad byl ověřen v interaktivní konzoli a odráží skutečnou datovou strukturu programu.

---

## 1. Znát terén

Konzole injektuje tři globální objekty: `app` (hlavní okno), `graph` (scéna/diagram) a `selected_node` (aktuálně vybraný uzel na plátně). Všechny příkazy vycházejí z těchto tří.

### Zobrazit všechny uzly

```python
>>> graph.nodes
```

**Příklad výstupu:**
```
Nody ve scéně:
  [0] Pokročilý generátor signálů (typ: SignalSourceNode, kategorie: Sources)
  [1] FFT (typ: FFTNode, kategorie: Processing)
```

Index v hranatých závorkách (`[0]`, `[1]`) je váš hlavní způsob přístupu k uzlu. Pořadí odpovídá pořadí vytvoření na plátně.

#### Alternativa: spočítat uzly nebo filtrovat podle typu

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### Zobrazit všechna připojení

```python
>>> graph.connections
```

**Příklad výstupu:**
```
Připojení ve scéně:
  [0] Pokročilý generátor signálů (out) → FFT (input)
```

Výstup zobrazuje název zdrojového uzlu, výstupní port, šipku, cílový uzel a vstupní port. Pokud připojení nevidíte, tok nebude možné spustit.

#### Alternativa: zobrazit připojení jednoho uzlu

```python
>>> selected_node.connectors
```

### Zobrazit vybraný uzel

Klikněte na uzel na plátně a poté spusťte:

```python
>>> selected_node
```

**Příklad výstupu:**
```
Uzel: FFT
  Typ: FFTNode
  Kategorie: Processing
  Porty: ['input', 'output', 'magnitude', 'phase']
```

> **💡 Poznámka:** Pokud není vybrán žádný uzel, `selected_node` má hodnotu `None`. Výběr uzlu také automaticky aktualizuje boční tabulku parametrů.

#### Alternativa: vybrat uzel pomocí kódu

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. Manipulace s uzly a připojeními bez myši

### Vytvořit nový uzel

Musíte znát přesný název třídy uzlu (stejně jako v katalogu). Argumenty jsou: `(typ, x, y)`.

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

Uzel se objeví na plátně na souřadnicích (300, 200). Pokud přesný název neznáte, vypište kategorie (viz sekce 7).

#### Alternativa: vytvořit více uzlů najednou

```python
>>> for i, typ in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(typ, 100 + i*200, 300)
```

### Ručně připojit uzly

Syntaxe: `graph.connect_nodes(zdroj, cíl, 'výstupní_port', 'vstupní_port')`. Porty závisí na každém uzlu; nikdy nepředpokládejte jejich názvy.

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 Poznámka:** Před připojením vždy zkontrolujte `graph.nodes[N].PORTS`. Uzel FFT má `'input'` a `'magnitude'`; generátor má `'output'`.

#### Alternativa: připojit na výchozí port

Pokud neznáte přesný název vstupního portu, některé uzly akceptují `None` pro použití prvního dostupného:

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### Odstranit uzel

```python
>>> graph.remove_node(graph.nodes[2])
```

Odstraní uzel a všechna jeho přidružená připojení. Indexy v `graph.nodes` se přeuspořádají, proto neukládejte staré reference.

#### Alternativa: odstranit všechny uzly kategorie

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### Zobrazit porty uzlu

```python
>>> graph.nodes[1].PORTS
```

**Příklad výstupu:**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

Klíč je název portu (řetězec). Hodnota je n-tice s vizuální pozicí. Pro připojení vás zajímají pouze klíče.

#### Alternativa: zobrazit porty jako jednoduchý seznam

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. Spustit tok a zobrazit výsledky

### Spustit celý graf

```python
>>> graph.execute_flow()
```

Tato metoda patří diagramu (`graph`), nikoli hlavnímu oknu. Projde všechny uzly v topologickém pořadí, spustí každý a uloží výsledky do mezipaměti. Nic nevrací; data zůstávají uložena interně.

#### Alternativa: vynutit výpočet konkrétní větve

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

To přepočítá celý strom nadřazených uzlů uvedeného uzlu a vrátí výsledek přímo, aniž by upravovalo globální mezipaměť.

### Zobrazit data v mezipaměti uzlu

Pokud potřebujete přístup k zpracovaným datům konkrétního uzlu, můžete to udělat dvěma způsoby:

#### Přímý způsob (podle objektu)
```python
>>> graph.node_values[graph.nodes[1]]
```

**Příklad výstupu:**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ Varování:** Tato forma může selhat s `KeyError`, pokud se pořadí uzlů ve scéně změnilo (např. při odstranění nebo přidání uzlů) nebo pokud instance objektu přesně neodpovídá uloženému klíči ve slovníku.

#### Alternativní způsob (podle pozice)
```python
>>> list(graph.node_values.values())[1]
```

Tato forma je **stabilnější**, protože nezávisí na přesné identitě objektu. Pořadí hodnot odpovídá sekvenci, v jaké byly uzly spuštěny během posledního `graph.execute_flow()`. Index `[1]` odpovídá druhému uzlu v této sekvenci.

> **💡 Poznámka:** Pokud chcete vidět index každého uzlu v pořadí spuštění, můžete použít:
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ Pozor:** `graph.node_values` nevrací vždy přímo pole. Pro zpracovací uzly (FFT, filtry atd.) vrací **slovník**, kde každý klíč je výstupní port. Pro zdrojové uzly vrací n-tici `(x, y)`.

#### Alternativa: zobrazit data všech uzlů na jednom řádku
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### Přístup k ose Y zdrojového uzlu

Zdrojové uzly (SignalSourceNode, FileInputNode atd.) při spuštění vracejí n-tici `(čas, signál)`. Pro získání pouze osy Y:

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**Příklad výstupu:**
```
Scalar NumPy (float64): 1.0
```

#### Alternativa: získat osu X (čas)

```python
>>> x = result[0]
>>> x[:5]
```

### Přiřazení v Pythonu: životně důležitý detail

V Pythonu jsou přiřazení (`=`) **příkazy**, nikoli výrazy. Konzole po `x, y = ...` nic nevypíše, protože není žádná návratová hodnota.

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

Pro ověření, že to fungovalo, vyhodnoťte proměnnou na následujícím řádku:

```python
>>> x
>>> y.shape
```

Nebo použijte `;` pro zřetězení výrazu na stejném řádku:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

Nebo použijte explicitní `print()`:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. Kreslení na hlavní graf

### Vyčistit graf

```python
>>> app.plot_widget.clear_plot()
```

#### Alternativa: vyčistit a okamžitě překreslit

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### Vykreslit libovolný signál z konzole

Můžete vytvářet pole pomocí NumPy a posílat je přímo do vykreslovacího widgetu, aniž byste procházeli jakýmkoli uzlem.

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### Alternativa: vykreslit součet sinusů

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### Vykreslit výsledek zdrojového uzlu

Protože zdrojový uzel vrací `(x, y)`, můžete jej přímo rozbalit:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### Vykreslit výsledek FFT uzlu (více portů)

Uzly s více výstupy (FFT, časově-frekvenční analýza atd.) nevracejí jednoduchou n-tici. Vrací `dict`, kde každý klíč je výstupní port.

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**Příklad výstupu:**
```
dict_keys(['output', 'magnitude', 'phase'])
```

Všimněte si, že `'output'` může být `None`, pokud uzel nemá generický port. Užitečné výstupy jsou `'magnitude'` a `'phase'`, které jsou opět n-ticemi `(frekvence, hodnoty)`:

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### Alternativa: vykreslit fázi místo magnitudy

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### Alternativa: překrýt dva signály

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # jiný uzel
>>> app.plot_widget.plot_waveform(x2, y2)  # překryje se
```

> **💡 Poznámka:** Pokud se pokusíte provést `x, y = result` s dict, Python vyvolá `ValueError: too many values to unpack`. Před rozbalením vždy zkontrolujte `type(result)` a `result.keys()`.

---

## 5. Úprava aplikace za běhu

### Aktualizovat boční tabulku parametrů

Pokud upravíte parametr pomocí kódu a chcete, aby se změna odrazila v boční tabulce:

```python
>>> app.workspace_table.populate()
```

Tato metoda nepřijímá žádné argumenty. Aktualizuje tabulku aktuálními hodnotami vybraného uzlu.

#### Alternativa: vynutit výběr jiného uzlu a obnovit

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### Přidat uzel z panelu nástrojů

```python
>>> app.add_node('SignalSourceNode')
```

Je to ekvivalent stisknutí tlačítka "+" na panelu nástrojů. Uzel se umístí na předdefinovanou pozici na plátně.

### Změnit název okna

`setWindowTitle` je nativní metoda Qt. Funguje, ale mějte na paměti, že aplikace může mít časovač nebo událost, která volá `update_title()` a automaticky jej přepíše.

```python
>>> app.setWindowTitle('Mé signálové laboratoře')
>>> app.windowTitle()
```

Pro obnovení "oficiálního" názvu, který aplikace vypočítá ze svého interního stavu (název projektu, soubor atd.):

```python
>>> app.update_title()
```

#### Alternativa: název s názvem projektu

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. Navigace a produktivita v konzoli

Konzole není jen `print()`. Má historii, automatické doplňování a víceřádkové bloky.

| Klávesa / Příkaz           | Akce                                                         |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | Procházení historie příkazů                |
| `Tab`                     | Automatické doplňování proměnných, atributů a metod namespace |
| `Ctrl + L`                | Vyčistit celou konzoli (smaže text, ne stav Pythonu)  |
| `if`, `for`, `def`, `class` | Výzva se změní z `>>>` na `...` pro víceřádkové bloky     |
| `Ctrl+C` (ve výběru)   | Kopírovat text z konzole                                     |
| `Ctrl+A`                  | Vybrat veškerý obsah                                  |

> **💡 Poznámka:** Automatické doplňování používá `rlcompleter` a rozpozná celý injektovaný namespace (`app`, `graph`, `selected_node`) plus jakoukoli proměnnou, kterou definujete v relaci.

---

## 7. Pokročilé recepty

### Změnit interní parametr uzlu

Parametry uzlů nejsou ploché atributy. Jsou vnořeny ve slovníku `params`, který má podsekce jako `'preset'`, `'formula'` nebo `'advanced'`. Nikdy nedělejte `nuzel.amplitude = 3.0`; to vytvoří nový atribut objektu, ale nezmění skutečný parametr.

#### Případ A: změnit preset (sinus, obdélník atd.)

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Případ B: použít vlastní vzorec

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

Výraz používá `t` jako časovou proměnnou. Hodnoty v `vars` jsou symboly, na které můžete ve vzorci odkazovat. Pokud vynecháte `vars`, uzel použije výchozí hodnoty a vzorec nemusí změnu odrážet.

#### Případ C: změnit pokročilé parametry (vzorkovací frekvence, délka)

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Případ D: změnit parametr negenerátorového uzlu (např. FFT)

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 Poznámka:** `getattr(obj, '_generate_signal', lambda: None)()` je bezpečný vzor: pokud metoda existuje (generátorové uzly), zavolá ji; pokud ne, nic neudělá a nevyvolá chybu. Pro zpracovací uzly stačí pouze `graph.execute_flow()`.

### Vypsat všechny dostupné kategorie uzlů

Import načte katalog, ale automaticky jej nezobrazí. Pamatujte, že v Pythonu úspěšný import nic nevypíše; musíte vyhodnotit objekt.

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

Pro čitelné shrnutí:

```python
>>> for cat, nody in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(nody)} nody")
```

#### Alternativa: vypsat názvy uzlů podle kategorie

```python
>>> {cat: [n.__name__ for n in nody] for cat, nody in NODE_CATEGORIES.items()}
```

### Zobrazit nápovědu libovolné metody

```python
>>> help(graph.connect_nodes)
```

Docstring se zobrazí přímo v konzoli. Je to užitečné pro zjištění argumentů metody bez otevření zdrojového kódu.

#### Alternativa: zobrazit filtrované atributy

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. Co dělat, když něco selže?

- **Chyba v červené v konzoli:** traceback se zobrazí celý. Aplikace se nezavře; můžete příkaz opravit a zkusit znovu.
- **Rozhraní se zasekne:** pravděpodobně jste napsali nekonečnou smyčku. Konzole běží ve vlákně odděleném od GUI, ale pokud smyčka ovlivňuje GUI vlákno, restartujte aplikaci.
- **Neočekávané `None`:** pokud uzel vrací `None` místo dat, ověřte, že je připojen nadřazeně (`graph.connections`) a že byl tok spuštěn (`graph.execute_flow()`).
- **`ValueError: too many values to unpack`:** pokoušíte se rozbalit dict jako n-tici. Nejprve použijte `result.keys()`.
- **`ValueError: not enough values to unpack`:** očekáváte 2 hodnoty, ale uzel vrací 1 (dict) nebo 3 (spektrogram). Před rozbalením zkontrolujte `type(result)`.
- **`AttributeError`:** objekt nemá tento atribut. Pro zjištění správného názvu použijte `dir(obj)` nebo `[a for a in dir(obj) if 'slovo' in a.lower()]`.
- **Nic se nestane po spuštění:** zkontrolujte, že existuje alespoň jeden zdrojový uzel připojený do řetězce a že byl zavolán `graph.execute_flow()`. Zpracovací uzly samy o sobě data negenerují.
- **Graf se nezmění:** ujistěte se, že po úpravě parametrů zavoláte `graph.execute_flow()`. Pouhá změna `params` automaticky nepřepočítává.

---

© 2026 FloWorks — Signálová laboratoř
