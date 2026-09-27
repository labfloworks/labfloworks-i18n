# 🧪 Tutorial Interattivo della Console FloWorks

Benvenuti nel laboratorio sperimentale di FloWorks. Questa sezione è per utenti più avanzati; si tratta di un terminale **Python** per controllare tutto ciò associato alla tela, ovvero i nodi e le loro connessioni, in modo sequenziale e tramite righe di codice. È un terminale collegato al programma che può controllarlo e determinare comportamenti o routine per utenti più esigenti.

Questa guida ti mostrerà **passo dopo passo** come controllare e analizzare i tuoi diagrammi di flusso senza toccare il mouse. Ogni esempio è stato validato nella console interattiva e riflette la struttura dati reale del programma.

---

## 1. Conoscere il terreno

La console inietta tre oggetti globali: `app` (finestra principale), `graph` (scena/diagramma) e `selected_node` (nodo attualmente selezionato sulla tela). Tutti i comandi partono da questi tre.

### Visualizzare tutti i nodi

```python
>>> graph.nodes
```

**Esempio di output:**
```
Nodi nella scena:
  [0] Generatore di segnali Avanzato (tipo: SignalSourceNode, categoria: Sources)
  [1] FFT (tipo: FFTNode, categoria: Processing)
```

L'indice tra parentesi quadre (`[0]`, `[1]`) è il tuo modo principale di accedere a un nodo. L'ordine è quello della creazione sulla tela.

#### Alternativa: contare i nodi o filtrare per tipo

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### Visualizzare tutte le connessioni

```python
>>> graph.connections
```

**Esempio di output:**
```
Connessioni nella scena:
  [0] Generatore di segnali Avanzato (out) → FFT (input)
```

L'output mostra il nome del nodo origine, la porta di uscita, la freccia, il nodo destinazione e la porta di ingresso. Se una connessione non appare, il flusso non potrà essere eseguito.

#### Alternativa: visualizzare le connessioni di un solo nodo

```python
>>> selected_node.connectors
```

### Visualizzare il nodo selezionato

Fai clic su un nodo sulla tela e poi esegui:

```python
>>> selected_node
```

**Esempio di output:**
```
Nodo: FFT
  Tipo: FFTNode
  Categoria: Processing
  Porte: ['input', 'output', 'magnitude', 'phase']
```

> **💡 Nota:** Se non è selezionato alcun nodo, `selected_node` vale `None`. Selezionare un nodo aggiorna anche automaticamente la tabella dei parametri laterale.

#### Alternativa: selezionare un nodo tramite codice

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. Manipolare nodi e connessioni senza mouse

### Creare un nuovo nodo

Devi conoscere il nome esatto della classe del nodo (come nel catalogo). Gli argomenti sono: `(tipo, x, y)`.

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

Il nodo appare sulla tela alle coordinate (300, 200). Se non conosci il nome esatto, elenca le categorie (vedi sezione 7).

#### Alternativa: creare più nodi contemporaneamente

```python
>>> for i, tipo in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(tipo, 100 + i*200, 300)
```

### Connettere nodi manualmente

Sintassi: `graph.connect_nodes(origine, destinazione, 'porta_uscita', 'porta_ingresso')`. Le porte dipendono da ogni nodo; non assumere mai i nomi.

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 Nota:** Controlla sempre `graph.nodes[N].PORTS` prima di connettere. Un nodo FFT ha `'input'` e `'magnitude'`; un generatore ha `'output'`.

#### Alternativa: connettere alla porta predefinita

Se non conosci il nome esatto della porta di ingresso, alcuni nodi accettano `None` per usare la prima disponibile:

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### Eliminare un nodo

```python
>>> graph.remove_node(graph.nodes[2])
```

Elimina il nodo e tutte le sue connessioni associate. Gli indici in `graph.nodes` vengono riordinati, quindi non conservare riferimenti vecchi.

#### Alternativa: eliminare tutti i nodi di una categoria

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### Visualizzare le porte di un nodo

```python
>>> graph.nodes[1].PORTS
```

**Esempio di output:**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

La chiave è il nome della porta (stringa). Il valore è una tupla con la posizione visiva. Solo le chiavi ti interessano per la connessione.

#### Alternativa: visualizzare le porte come elenco semplice

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. Eseguire il flusso e visualizzare i risultati

### Eseguire l'intero grafo

```python
>>> graph.execute_flow()
```

Questo metodo appartiene al diagramma (`graph`), non alla finestra principale. Attraversa tutti i nodi in ordine topologico, esegue ciascuno e memorizza i risultati nella cache. Non restituisce nulla; i dati rimangono memorizzati internamente.

#### Alternativa: forzare il calcolo di un ramo specifico

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

Questo ricalcola l'intero albero a monte del nodo indicato e restituisce il risultato direttamente, senza modificare la cache globale.

### Visualizzare i dati in cache di un nodo

Se hai bisogno di accedere ai dati elaborati di un nodo specifico, puoi farlo in due modi:

#### Modo diretto (per oggetto)
```python
>>> graph.node_values[graph.nodes[1]]
```

**Esempio di output:**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ Avvertenza:** Questa forma può fallire con `KeyError` se l'ordine dei nodi nella scena è cambiato (ad esempio eliminando o aggiungendo nodi) o se l'istanza dell'oggetto non corrisponde esattamente alla chiave salvata nel dizionario.

#### Modo alternativo (per posizione)
```python
>>> list(graph.node_values.values())[1]
```

Questa forma è **più stabile** perché non dipende dall'identità esatta dell'oggetto. L'ordine dei valori segue la sequenza in cui i nodi sono stati eseguiti durante l'ultimo `graph.execute_flow()`. L'indice `[1]` corrisponde al secondo nodo in quella sequenza.

> **💡 Nota:** Se vuoi vedere l'indice di ogni nodo nell'ordine di esecuzione, puoi usare:
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ Attenzione:** `graph.node_values` non restituisce sempre direttamente un array. Per nodi di elaborazione (FFT, filtri, ecc.) restituisce un **dizionario** dove ogni chiave è una porta di uscita. Per nodi sorgente, restituisce una tupla `(x, y)`.

#### Alternativa: visualizzare i dati di tutti i nodi in una riga
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### Accedere all'asse Y di un nodo sorgente

I nodi generatori (SignalSourceNode, FileInputNode, ecc.) restituiscono una tupla `(tempo, segnale)` all'esecuzione. Per ottenere solo l'asse Y:

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**Esempio di output:**
```
Scalar NumPy (float64): 1.0
```

#### Alternativa: ottenere l'asse X (tempo)

```python
>>> x = result[0]
>>> x[:5]
```

### Assegnazioni in Python: un dettaglio vitale

In Python, le assegnazioni (`=`) sono **istruzioni**, non espressioni. La console non stampa nulla dopo `x, y = ...` perché non c'è un valore di ritorno.

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

Per verificare che ha funzionato, valuta la variabile nella riga successiva:

```python
>>> x
>>> y.shape
```

Oppure usa `;` per concatenare un'espressione sulla stessa riga:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

Oppure usa `print()` esplicito:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. Disegnare sul grafico principale

### Pulire il grafico

```python
>>> app.plot_widget.clear_plot()
```

#### Alternativa: pulire e ridisegnare immediatamente

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### Disegnare un segnale arbitrario dalla console

Puoi creare array con NumPy e inviarli direttamente al widget di plotting, senza passare per alcun nodo.

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### Alternativa: disegnare una somma di seni

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### Disegnare il risultato di un nodo sorgente

Poiché il nodo sorgente restituisce `(x, y)`, puoi decomprimerlo direttamente:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### Disegnare il risultato di un nodo FFT (porte multiple)

I nodi con uscite multiple (FFT, analisi tempo-frequenza, ecc.) non restituiscono una semplice tupla. Restituiscono un `dict`, dove ogni chiave è una porta di uscita.

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**Esempio di output:**
```
dict_keys(['output', 'magnitude', 'phase'])
```

Nota che `'output'` può essere `None` se il nodo non ha una porta generica. Le uscite utili sono `'magnitude'` e `'phase'`, che a loro volta sono tuple `(frequenze, valori)`:

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### Alternativa: disegnare la fase invece della magnitudine

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### Alternativa: sovrapporre due segnali

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # altro nodo
>>> app.plot_widget.plot_waveform(x2, y2)  # si sovrappone
```

> **💡 Nota:** Se provi a fare `x, y = result` con un dict, Python lancerà `ValueError: too many values to unpack`. Controlla sempre con `type(result)` e `result.keys()` prima di decomprimere.

---

## 5. Modificare l'applicazione al volo

### Aggiornare la tabella dei parametri laterale

Se modifichi un parametro tramite codice e vuoi che la tabella laterale rifletta la modifica:

```python
>>> app.workspace_table.populate()
```

Questo metodo non accetta argomenti. Aggiorna la tabella con i valori attuali del nodo selezionato.

#### Alternativa: forzare la selezione di un altro nodo e aggiornare

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### Aggiungere un nodo dalla barra degli strumenti

```python
>>> app.add_node('SignalSourceNode')
```

È equivalente a premere il pulsante "+" sulla barra degli strumenti. Il nodo viene posizionato in una posizione predefinita sulla tela.

### Cambiare il titolo della finestra

`setWindowTitle` è il metodo nativo di Qt. Funziona, ma tieni presente che l'applicazione potrebbe avere un timer o un evento che chiama `update_title()` e lo sovrascrive automaticamente.

```python
>>> app.setWindowTitle('Il mio laboratorio di segnali')
>>> app.windowTitle()
```

Per ripristinare il titolo "ufficiale" che l'app calcola dal suo stato interno (nome progetto, file, ecc.):

```python
>>> app.update_title()
```

#### Alternativa: titolo con nome progetto

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. Navigazione e produttività nella console

La console non è solo un `print()`. Ha cronologia, autocompletamento e blocchi multilinea.

| Tasto / Comando           | Azione                                                         |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | Navigare nella cronologia dei comandi eseguiti                |
| `Tab`                     | Autocompletamento variabili, attributi e metodi del namespace     |
| `Ctrl + L`                | Pulire l'intera console (cancella testo, non lo stato Python)  |
| `if`, `for`, `def`, `class` | Il prompt cambia da `>>>` a `...` per blocchi multilinea     |
| `Ctrl+C` (in selezione)   | Copiare testo dalla console                                     |
| `Ctrl+A`                  | Selezionare tutto il contenuto                                  |

> **💡 Nota:** L'autocompletamento usa `rlcompleter` e riconosce l'intero namespace iniettato (`app`, `graph`, `selected_node`) più qualsiasi variabile definita nella sessione.

---

## 7. Ricette avanzate

### Modificare un parametro interno di un nodo

I parametri dei nodi non sono attributi piani. Sono annidati nel dizionario `params`, che a sua volta ha sottosezioni come `'preset'`, `'formula'` o `'advanced'`. Non fare mai `nodo.amplitude = 3.0`; questo crea un nuovo attributo sull'oggetto ma non modifica il parametro reale.

#### Caso A: modificare un preset (seno, quadra, ecc.)

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Caso B: usare una formula personalizzata

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

L'espressione usa `t` come variabile temporale. I valori in `vars` sono i simboli a cui puoi fare riferimento nella formula. Se ometti `vars`, il nodo userà valori predefiniti e la formula potrebbe non riflettere la modifica.

#### Caso C: cambiare parametri avanzati (frequenza di campionamento, durata)

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Caso D: cambiare parametro di un nodo non generatore (es. FFT)

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 Nota:** `getattr(obj, '_generate_signal', lambda: None)()` è un pattern sicuro: se il metodo esiste (nodi generatori), lo chiama; altrimenti non fa nulla e non lancia errore. Per nodi di elaborazione, solo `graph.execute_flow()` è sufficiente.

### Elencare tutte le categorie di nodi disponibili

L'import carica il catalogo, ma non lo mostra automaticamente. Ricorda che in Python un import riuscito non stampa nulla; devi valutare l'oggetto.

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

Per un riepilogo leggibile:

```python
>>> for cat, nodi in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(nodi)} nodi")
```

#### Alternativa: elencare nomi dei nodi per categoria

```python
>>> {cat: [n.__name__ for n in nodi] for cat, nodi in NODE_CATEGORIES.items()}
```

### Visualizzare l'aiuto di qualsiasi metodo

```python
>>> help(graph.connect_nodes)
```

La docstring appare direttamente nella console. È utile per scoprire quali argomenti si aspetta un metodo senza aprire il codice sorgente.

#### Alternativa: visualizzare attributi filtrati

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. Cosa fare se qualcosa va storto?

- **Errore in rosso nella console:** il traceback viene mostrato completo. L'applicazione non si chiude; puoi correggere il comando e riprovare.
- **L'interfaccia si blocca:** probabilmente hai scritto un ciclo infinito. La console gira in un thread separato, ma se il ciclo influenza il thread GUI, riavvia l'applicazione.
- **`None` inatteso:** se un nodo restituisce `None` invece di dati, verifica che sia connesso a monte (`graph.connections`) e che il flusso sia stato eseguito (`graph.execute_flow()`).
- **`ValueError: too many values to unpack`:** stai cercando di decomprimere un dict come se fosse una tupla. Usa prima `result.keys()`.
- **`ValueError: not enough values to unpack`:** ti aspetti 2 valori ma il nodo ne restituisce 1 (dict) o 3 (spettrogramma). Ispeziona con `type(result)` prima di decomprimere.
- **`AttributeError`:** l'oggetto non ha quell'attributo. Usa `dir(obj)` o `[a for a in dir(obj) if 'parola' in a.lower()]` per scoprire il nome corretto.
- **Non succede nulla all'esecuzione:** verifica che ci sia almeno un nodo sorgente connesso alla catena e che sia stato chiamato `graph.execute_flow()`. I nodi di elaborazione non generano dati da soli.
- **Il plot non cambia:** assicurati di chiamare `graph.execute_flow()` dopo aver modificato i parametri. Solo cambiare `params` non ricalcola automaticamente.

---

© 2026 FloWorks — Laboratorio di Segnali
