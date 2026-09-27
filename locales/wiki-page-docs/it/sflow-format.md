---
title: Formato di File .sflow
description: Specifica tecnica, struttura interna e guida d'uso dello standard di scambio FloWorks
---

# 📄 Formato di file `.sflow`

Il formato `.sflow` è lo standard nativo di scambio e persistenza di **FloWorks**. Permette di impacchettare un flusso di lavoro completo in un unico file che include la topologia del grafo, i parametri dei nodi, i dati elaborati e le note adesive, facilitando la condivisione, l'archiviazione o la riproduzione deterministica degli esperimenti.

---

## 📦 Cos'è un file `.sflow`?

Un file `.sflow` è, in sostanza, **un file ZIP rinominato**. Cambiando la sua estensione in `.zip`, è possibile ispezionarne il contenuto con qualsiasi gestore di file o strumento da riga di comando.

La sua struttura interna minima consiste in:

| Componente | Descrizione |
|------------|-------------|
| `diagram.json` | Manifesto principale: definisce nodi, connessioni, vista, note adesive e metadati di serializzazione. |
| `data/` | Cartella con i dati di ogni nodo in formato `.npy` (array binari NumPy). |
| `metadata.json` *(opzionale)* | Informazioni complementari: autore, versione di FloWorks, descrizione ed etichette. |

=== "🌳 Struttura Visiva"
    ```text
    mi-flujo.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (opzionale, per ScriptNode persist)
    ```

---

## 🧩 `diagram.json` – Il Cuore del Flusso

Questo file JSON descrive la topologia completa, la posizione degli elementi sulla tela e lo stato della vista al momento del salvataggio.

### Esempio Minimo
```json
{
  "nodes": [
    {
      "id": "n1",
      "type": "OscilloscopeNode",
      "pos": [150, 200],
      "params": { "channel": "primary", "simulation": false }
    },
    {
      "id": "n2",
      "type": "GraphExporterNode",
      "pos": [450, 200],
      "params": { "theme": "dark", "export_format": "png" }
    }
  ],
  "connections": [
    {
      "from": "n1",
      "to": "n2",
      "from_port": "out",
      "to_port": "data_in"
    }
  ],
  "viewport": { "x": 0, "y": 0, "scale": 1.0 },
  "stickers": [
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "Controlla la soglia", "user_modified": true }
  ]
}
```

### Campi Principali
| Campo | Tipo | Descrizione |
|-------|------|-------------|
| `nodes` | `Array` | Lista di oggetti `{id, type, pos, params}`. `type` deve coincidere con `node_registry.py`. |
| `connections` | `Array` | Lista di connessioni `{from, to, from_port, to_port}`. Le porte sono stringhe, non indici. |
| `viewport` | `Object` | `(x, y, scale)` per ripristinare esattamente la posizione e lo zoom della tela. |
| `stickers` | `Array` | Note adesive serializzate con coordinate normalizzate a 96 dpi. |

!!! tip "Serializzazione dei Nodi"
    I parametri specifici di ogni nodo sono gestiti tramite `SerializableMixin`. Vengono salvati solo gli attributi dichiarati in `SERIALISABLE = [...]`. I nodi non registrati nel sistema vengono omessi automaticamente durante il caricamento.

---

## 💾 `data/` – Dati Elaborati e Array NumPy

Quando un flusso viene eseguito, i nodi possono memorizzare i propri risultati in file `.npy` all'interno di questa cartella.

- Il nome del file di solito coincide con l'`id` del nodo o con riferimenti interni.
- Gli array sono memorizzati in formato binario NumPy, **preservando rigorosamente la dimensionalità originale** (1D, 2D, 3D, ecc.). Il motore non applica mai `flatten()`.
- In `diagram.json`, i dati sono referenziati con il prefisso `__npy__:`:
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- Se un nodo non produce dati o è configurato per non persistere, il file corrispondente può essere omesso.

??? note "Compatibilità Esterna"
    I file `.npy` sono universali nell'ecosistema Python. È possibile leggerli al di fuori di FloWorks con:
    ```python
    import numpy as np
    dati = np.load("data/node_1.npy")
    print(dati.shape)
    ```

---

## 🏷️ `metadata.json` (Opzionale)

Contiene informazioni descrittive che non influenzano l'esecuzione, ideale per tracciabilità e gestione dei progetti:

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "Analisi delle vibrazioni in motore trifase",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["ingegneria", "vibrazioni", "FFT", "multicanale"]
}
```

---

## 🔄 Processo di Salvataggio e Caricamento

FloWorks implementa un meccanismo robusto per garantire l'integrità dei dati:

1. **Salvataggio:**
   - Il grafo viene attraversato e i nodi serializzati tramite `SerializableMixin`.
   - Gli array vengono estratti in `data/` e referenziati in JSON con `__npy__:`.
   - Il tutto viene impacchettato in un ZIP con estensione `.sflow`.
2. **Caricamento Sicuro:**
   - Viene creato un **backup temporaneo in memoria** del diagramma attuale.
   - Il nuovo `.sflow` viene estratto e analizzato.
   - Se si verifica un errore (JSON non valido, nodi mancanti, corruzione `.npy`), **il backup viene ripristinato automaticamente** senza perdita di lavoro.
3. **ScriptNode Speciale:**
   - Salva `script`, `params`, `dynamic_inputs`, `dynamic_outputs`, `persist` e `python_path`.
   - Al caricamento, ricompila il codice, ricostruisce le porte dinamiche e ripristina automaticamente lo stato `persist`.
4. **StickyNotes e DPI:**
   - Le coordinate e le dimensioni vengono normalizzate a **96 dpi** al salvataggio.
   - Al caricamento, vengono scalate al DPI del monitor corrente, garantendo coerenza visiva tra diverse risoluzioni.

---

## 🛠️ Uso Esterno e Automazione

Il formato `.sflow` è progettato per essere trasparente e programmatico. È possibile leggerlo o generarlo da script esterni:

=== "🐍 Python (Lettura)"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("mi-flujo.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        dati_n1 = np.load(z.open("data/node_1.npy"))
        print(f"Nodi: {len(graph['nodes'])}")
        print(f"Dati: {dati_n1.shape}")
    ```

=== "📤 Python (Creazione Base)"
    ```python
    import zipfile
    import json
    import numpy as np

    graph = {
        "nodes": [{"id": "gen", "type": "GeneratorNode", "pos": [100, 100], "params": {}}],
        "connections": [],
        "viewport": {"x": 0, "y": 0, "scale": 1.0}
    }

    with zipfile.ZipFile("nuevo.sflow", "w", zipfile.ZIP_DEFLATED) as z:
        z.writestr("diagram.json", json.dumps(graph, indent=2))
        z.writestr("data/gen.npy", np.array([1.0, 2.0, 3.0]))
    ```

---

## 🔮 Compatibilità ed Estensibilità Futura

Il formato `.sflow` segue i principi di **progettazione estensibile e retrocompatibile**:

- ✅ **Nuove sezioni:** Le versioni future possono aggiungere cartelle come `thumbnails/`, `logs/` o `plugins/` senza rompere i caricatore vecchi.
- ✅ **Campi opzionali:** Il parser ignora chiavi sconosciute in `diagram.json`, permettendo di aggiungere metadati sperimentali.
- ✅ **Versionamento:** Il campo `floworks_version` in `metadata.json` permette all'applicazione di applicare migrazioni automatiche se il formato evolve.

!!! warning "Regola d'Oro"
    Non modificare mai manualmente `diagram.json` mentre l'applicazione è aperta. Il sistema dipende dalla coerenza tra topologia, array e stato della vista. Usare sempre i flussi nativi di salvataggio/caricamento.

---

## 📚 Risorse Correlate
- [🗺️ Mappa del Codice e dell'Architettura](architecture-ii.md) → Come `file_io.py` e `SerializableMixin` gestiscono il formato.
- [📦 Guida al Build Portatile](guia-ejecutable-portable.md) → Impacchettamento e percorsi sicuri per le risorse.
- [🧩 Riferimento Tecnico dei Nodi](node-reference.md) → Contratti di serializzazione per tipo di nodo.
