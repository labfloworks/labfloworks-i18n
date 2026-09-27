# 🧪 Interaktywny samouczek konsoli FloWorks

Witaj w laboratorium eksperymentalnym FloWorks. Ta sekcja jest dla bardziej zaawansowanych użytkowników; jest to terminal **Python** do sterowania wszystkim związanym z płótnem, czyli węzłami i ich połączeniami, w sposób sekwencyjny i za pomocą linii kodu. Jest to terminal połączony z programem, który może nim sterować i określać zachowania lub procedury dla bardziej wymagających użytkowników.

Ten przewodnik **krok po kroku** pokaże, jak sterować i analizować schematy blokowe bez użycia myszy. Każdy przykład został zweryfikowany w interaktywnej konsoli i odzwierciedla rzeczywistą strukturę danych programu.

---

## 1. Poznać teren

Konsola wstrzykuje trzy globalne obiekty: `app` (główne okno), `graph` (scena/diagram) i `selected_node` (aktualnie wybrany węzeł na płótnie). Wszystkie polecenia wychodzą od tych trzech.

### Zobaczyć wszystkie węzły

```python
>>> graph.nodes
```

**Przykładowe wyjście:**
```
Węzły na scenie:
  [0] Zaawansowany generator sygnałów (typ: SignalSourceNode, kategoria: Sources)
  [1] FFT (typ: FFTNode, kategoria: Processing)
```

Indeks w nawiasach kwadratowych (`[0]`, `[1]`) to Twój główny sposób dostępu do węzła. Kolejność odpowiada kolejności tworzenia na płótnie.

#### Alternatywa: policzyć węzły lub filtrować według typu

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### Zobaczyć wszystkie połączenia

```python
>>> graph.connections
```

**Przykładowe wyjście:**
```
Połączenia na scenie:
  [0] Zaawansowany generator sygnałów (out) → FFT (input)
```

Wyjście pokazuje nazwę węzła źródłowego, port wyjściowy, strzałkę, węzeł docelowy i port wejściowy. Jeśli połączenie nie pojawia się, przepływ nie będzie mógł zostać wykonany.

#### Alternatywa: zobaczyć połączenia jednego węzła

```python
>>> selected_node.connectors
```

### Zobaczyć wybrany węzeł

Kliknij węzeł na płótnie, a następnie wykonaj:

```python
>>> selected_node
```

**Przykładowe wyjście:**
```
Węzeł: FFT
  Typ: FFTNode
  Kategoria: Processing
  Porty: ['input', 'output', 'magnitude', 'phase']
```

> **💡 Uwaga:** Jeśli żaden węzeł nie jest wybrany, `selected_node` ma wartość `None`. Wybranie węzła automatycznie aktualizuje również boczną tabelę parametrów.

#### Alternatywa: wybrać węzeł za pomocą kodu

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. Manipulowanie węzłami i połączeniami bez myszy

### Utworzyć nowy węzeł

Musisz znać dokładną nazwę klasy węzła (tak jak w katalogu). Argumenty to: `(typ, x, y)`.

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

Węzeł pojawia się na płótnie na współrzędnych (300, 200). Jeśli nie znasz dokładnej nazwy, wypisz kategorie (zobacz sekcję 7).

#### Alternatywa: utworzyć kilka węzłów na raz

```python
>>> for i, typ in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(typ, 100 + i*200, 300)
```

### Ręcznie połączyć węzły

Składnia: `graph.connect_nodes(źródło, cel, 'port_wyjściowy', 'port_wejściowy')`. Porty zależą od każdego węzła; nigdy nie zakładaj ich nazw.

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 Uwaga:** Zawsze sprawdzaj `graph.nodes[N].PORTS` przed łączeniem. Węzeł FFT ma `'input'` i `'magnitude'`; generator ma `'output'`.

#### Alternatywa: połączyć do portu domyślnego

Jeśli nie znasz dokładnej nazwy portu wejściowego, niektóre węzły akceptują `None`, aby użyć pierwszego dostępnego:

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### Usunąć węzeł

```python
>>> graph.remove_node(graph.nodes[2])
```

Usuwa węzeł i wszystkie powiązane z nim połączenia. Indeksy w `graph.nodes` zostaną przeorganizowane, więc nie zapisuj starych referencji.

#### Alternatywa: usunąć wszystkie węzły kategorii

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### Zobaczyć porty węzła

```python
>>> graph.nodes[1].PORTS
```

**Przykładowe wyjście:**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

Klucz to nazwa portu (łańcuch). Wartość to krotka z pozycją wizualną. Do łączenia interesują Cię tylko klucze.

#### Alternatywa: zobaczyć porty jako prostą listę

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. Wykonać przepływ i zobaczyć wyniki

### Wykonać cały graf

```python
>>> graph.execute_flow()
```

Ta metoda należy do diagramu (`graph`), nie do głównego okna. Przechodzi przez wszystkie węzły w kolejności topologicznej, wykonuje każdy i buforuje wyniki. Nic nie zwraca; dane pozostają przechowywane wewnętrznie.

#### Alternatywa: wymusić obliczenie konkretnej gałęzi

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

To przelicza całe drzewo węzłów nadrzędnych wskazanego węzła i zwraca wynik bezpośrednio, bez modyfikowania globalnej pamięci podręcznej.

### Zobaczyć buforowane dane węzła

Jeśli potrzebujesz dostępu do przetworzonych danych konkretnego węzła, możesz to zrobić na dwa sposoby:

#### Sposób bezpośredni (po obiekcie)
```python
>>> graph.node_values[graph.nodes[1]]
```

**Przykładowe wyjście:**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ Ostrzeżenie:** Ta forma może zakończyć się `KeyError`, jeśli kolejność węzłów na scenie uległa zmianie (np. przy usuwaniu lub dodawaniu węzłów) lub jeśli instancja obiektu nie zgadza się dokładnie z kluczem zapisanym w słowniku.

#### Sposób alternatywny (po pozycji)
```python
>>> list(graph.node_values.values())[1]
```

Ta forma jest **bardziej stabilna**, ponieważ nie zależy od dokładnej tożsamości obiektu. Kolejność wartości odpowiada sekwencji, w jakiej węzły były wykonywane podczas ostatniego `graph.execute_flow()`. Indeks `[1]` odpowiada drugiemu węzłowi w tej sekwencji.

> **💡 Uwaga:** Jeśli chcesz zobaczyć indeks każdego węzła w kolejności wykonania, możesz użyć:
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ Uwaga:** `graph.node_values` nie zawsze zwraca bezpośrednio tablicę. Dla węzłów przetwarzających (FFT, filtry itp.) zwraca **słownik**, gdzie każdy klucz to port wyjściowy. Dla węzłów źródłowych zwraca krotkę `(x, y)`.

#### Alternatywa: zobaczyć dane wszystkich węzłów w jednej linii
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### Dostęp do osi Y węzła źródłowego

Węzły źródłowe (SignalSourceNode, FileInputNode itp.) przy wykonaniu zwracają krotkę `(czas, sygnał)`. Aby uzyskać tylko oś Y:

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**Przykładowe wyjście:**
```
Scalar NumPy (float64): 1.0
```

#### Alternatywa: uzyskać oś X (czas)

```python
>>> x = result[0]
>>> x[:5]
```

### Przypisania w Pythonie: żywotnie ważny szczegół

W Pythonie przypisania (`=`) to **instrukcje**, nie wyrażenia. Konsola nic nie wyświetla po `x, y = ...`, ponieważ nie ma wartości zwracanej.

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

Aby zweryfikować, że zadziałało, oceń zmienną w następnym wierszu:

```python
>>> x
>>> y.shape
```

Lub użyj `;` do połączenia wyrażenia w tym samym wierszu:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

Lub użyj jawnego `print()`:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. Rysowanie na głównym wykresie

### Wyczyścić wykres

```python
>>> app.plot_widget.clear_plot()
```

#### Alternatywa: wyczyścić i natychmiast przerysować

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### Narysować dowolny sygnał z konsoli

Możesz tworzyć tablice za pomocą NumPy i wysyłać je bezpośrednio do widżetu wykresu, bez przechodzenia przez żaden węzeł.

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### Alternatywa: narysować sumę sinusów

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### Narysować wynik węzła źródłowego

Ponieważ węzeł źródłowy zwraca `(x, y)`, możesz go bezpośrednio rozpakować:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### Narysować wynik węzła FFT (wiele portów)

Węzły z wieloma wyjściami (FFT, analiza czasowo-częstotliwościowa itp.) nie zwracają prostej krotki. Zwracają `dict`, gdzie każdy klucz to port wyjściowy.

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**Przykładowe wyjście:**
```
dict_keys(['output', 'magnitude', 'phase'])
```

Zauważ, że `'output'` może być `None`, jeśli węzeł nie ma generycznego portu. Przydatne wyjścia to `'magnitude'` i `'phase'`, które z kolei są krotkami `(częstotliwości, wartości)`:

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### Alternatywa: narysować fazę zamiast modułu

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### Alternatywa: nałożyć dwa sygnały

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # inny węzeł
>>> app.plot_widget.plot_waveform(x2, y2)  # nakłada się
```

> **💡 Uwaga:** Jeśli spróbujesz wykonać `x, y = result` ze słownikiem, Python zgłosi `ValueError: too many values to unpack`. Przed rozpakowaniem zawsze sprawdzaj `type(result)` i `result.keys()`.

---

## 5. Modyfikowanie aplikacji w locie

### Zaktualizować boczną tabelę parametrów

Jeśli zmodyfikujesz parametr za pomocą kodu i chcesz, aby zmiana była odzwierciedlona w tabeli bocznej:

```python
>>> app.workspace_table.populate()
```

Ta metoda nie przyjmuje argumentów. Odświeża tabelę aktualnymi wartościami wybranego węzła.

#### Alternatywa: wymusić wybór innego węzła i odświeżyć

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### Dodać węzeł z paska narzędzi

```python
>>> app.add_node('SignalSourceNode')
```

Jest to równoważne naciśnięciu przycisku "+" na pasku narzędzi. Węzeł jest umieszczany w domyślnej pozycji na płótnie.

### Zmienić tytuł okna

`setWindowTitle` to natywna metoda Qt. Działa, ale pamiętaj, że aplikacja może mieć timer lub zdarzenie wywołujące `update_title()`, który automatycznie go nadpisze.

```python
>>> app.setWindowTitle('Moje laboratorium sygnałowe')
>>> app.windowTitle()
```

Aby przywrócić "oficjalny" tytuł, który aplikacja oblicza ze swojego stanu wewnętrznego (nazwa projektu, plik itp.):

```python
>>> app.update_title()
```

#### Alternatywa: tytuł z nazwą projektu

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. Nawigacja i produktywność w konsoli

Konsola to nie tylko `print()`. Ma historię, autouzupełnianie i bloki wielowierszowe.

| Klawisz / Polecenie           | Akcja                                                         |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | Przeglądanie historii poleceń               |
| `Tab`                     | Autouzupełnianie zmiennych, atrybutów i metod namespace |
| `Ctrl + L`                | Wyczyścić całą konsolę (usuwa tekst, nie stan Pythona)  |
| `if`, `for`, `def`, `class` | Znak zachęty zmienia się z `>>>` na `...` dla bloków wielowierszowych |
| `Ctrl+C` (w zaznaczeniu)   | Kopiowanie tekstu z konsoli                                     |
| `Ctrl+A`                  | Zaznaczenie całej zawartości                                  |

> **💡 Uwaga:** Autouzupełnianie używa `rlcompleter` i rozpoznaje cały wstrzyknięty namespace (`app`, `graph`, `selected_node`) plus dowolną zmienną zdefiniowaną w sesji.

---

## 7. Zaawansowane przepisy

### Zmienić wewnętrzny parametr węzła

Parametry węzłów nie są płaskimi atrybutami. Są zagnieżdżone w słowniku `params`, który ma podsekcje takie jak `'preset'`, `'formula'` lub `'advanced'`. Nigdy nie rób `węzeł.amplitude = 3.0`; to tworzy nowy atrybut obiektu, ale nie modyfikuje rzeczywistego parametru.

#### Przypadek A: zmodyfikować preset (sinus, prostokąt itp.)

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Przypadek B: użyć własnego wzoru

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

Wyrażenie używa `t` jako zmiennej czasu. Wartoci w `vars` to symbole, do których możesz odwoływać się we wzorze. Jeśli pominiesz `vars`, węzeł użyje wartości domyślnych i wzór może nie odzwierciedlić zmiany.

#### Przypadek C: zmienić zaawansowane parametry (częstotliwość próbkowania, czas trwania)

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Przypadek D: zmienić parametr węzła niebędącego generatorem (np. FFT)

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 Uwaga:** `getattr(obj, '_generate_signal', lambda: None)()` to bezpieczny wzorzec: jeśli metoda istnieje (węzły generujące), wywołuje ją; jeśli nie, nic nie robi i nie zgłasza błędu. Dla węzłów przetwarzających wystarczy samo `graph.execute_flow()`.

### Wypisać wszystkie dostępne kategorie węzłów

Import ładuje katalog, ale nie wyświetla go automatycznie. Pamiętaj, że w Pythonie udany import nic nie wypisuje; musisz obliczyć obiekt.

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

Dla czytelnego podsumowania:

```python
>>> for cat, węzły in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(węzły)} węzłów")
```

#### Alternatywa: wypisać nazwy węzłów według kategorii

```python
>>> {cat: [n.__name__ for n in węzły] for cat, węzły in NODE_CATEGORIES.items()}
```

### Zobaczyć pomoc dowolnej metody

```python
>>> help(graph.connect_nodes)
```

Docstring pojawia się bezpośrednio w konsoli. Jest to przydatne do odkrywania argumentów metody bez otwierania kodu źródłowego.

#### Alternatywa: zobaczyć filtrowane atrybuty

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. Co zrobić, jeśli coś pójdzie nie tak?

- **Błąd w czerwonym w konsoli:** traceback wyświetla się w całości. Aplikacja się nie zamyka; możesz poprawić polecenie i spróbować ponownie.
- **Interfejs się zawiesza:** prawdopodobnie napisałeś nieskończoną pętlę. Konsola działa w osobnym wątku, ale jeśli pętla wpływa na wątek GUI, zrestartuj aplikację.
- **Nieoczekiwane `None`:** jeśli węzeł zwraca `None` zamiast danych, sprawdź, czy jest podłączony w górę strumienia (`graph.connections`) i czy przepływ został wykonany (`graph.execute_flow()`).
- **`ValueError: too many values to unpack`:** próbujesz rozpakować słownik jak krotkę. Użyj najpierw `result.keys()`.
- **`ValueError: not enough values to unpack`:** oczekujesz 2 wartości, ale węzeł zwraca 1 (dict) lub 3 (spektrogram). Sprawdź `type(result)` przed rozpakowaniem.
- **`AttributeError`:** obiekt nie ma tego atrybutu. Użyj `dir(obj)` lub `[a for a in dir(obj) if 'słowo' in a.lower()]` aby odkryć poprawną nazwę.
- **Nic się nie dzieje po wykonaniu:** sprawdź, czy istnieje co najmniej jeden węzeł źródłowy podłączony do łańcucha i czy zostało wywołane `graph.execute_flow()`. Węzły przetwarzające same z siebie nie generują danych.
- **Wykres się nie zmienia:** upewnij się, że po modyfikacji parametrów wywołujesz `graph.execute_flow()`. Samo zmienienie `params` nie przelicza automatycznie.

---

© 2026 FloWorks — Laboratorium Sygnałów
