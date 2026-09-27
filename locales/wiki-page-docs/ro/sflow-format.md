---
title: Format de fișier .sflow
description: Specificație tehnică, structură internă și ghid de utilizare al standardului de schimb FloWorks
---

# 📄 Format de fișier `.sflow`

Formatul `.sflow` este standardul nativ de schimb și persistență al **FloWorks**. Permite ambalarea unui flux de lucru complet într-un singur fișier care include topologia grafului, parametrii nodurilor, datele procesate și notele adezive, facilitând partajarea, arhivarea sau reproducerea deterministă a experimentelor.

---

## 📦 Ce este un fișier `.sflow`?

Un fișier `.sflow` este, în esență, **un fișier ZIP redenumit**. Schimbând extensia în `.zip`, puteți inspecta conținutul său cu orice manager de fișiere sau instrument de linie de comandă.

Structura sa internă minimă constă din:

| Componentă | Descriere |
|------------|-------------|
| `diagram.json` | Manifestul principal: definește nodurile, conexiunile, vizualizarea, notele adezive și metadatele de serializare. |
| `data/` | Folder cu datele fiecărui nod în format `.npy` (array-uri binare NumPy). |
| `metadata.json` *(opțional)* | Informații complementare: autor, versiunea FloWorks, descriere și etichete. |

=== "🌳 Structură Vizuală"
    ```text
    mi-flujo.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (opțional, pentru ScriptNode persist)
    ```

---

## 🧩 `diagram.json` – Inima Fluxului

Acest fișier JSON descrie topologia completă, poziția elementelor pe pânză și starea vizualizării în momentul salvării.

### Exemplu Minim
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
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "Verifică pragul", "user_modified": true }
  ]
}
```

### Câmpuri Principale
| Câmp | Tip | Descriere |
|-------|------|-------------|
| `nodes` | `Array` | Listă de obiecte `{id, type, pos, params}`. `type` trebuie să coincidă cu `node_registry.py`. |
| `connections` | `Array` | Listă de conexiuni `{from, to, from_port, to_port}`. Porturile sunt șiruri, nu indici. |
| `viewport` | `Object` | `(x, y, scale)` pentru a restabili exact poziția și zoom-ul pânzei. |
| `stickers` | `Array` | Note adezive serializate cu coordonate normalizate la 96 dpi. |

!!! tip "Serializarea Nodurilor"
    Parametrii specifici fiecărui nod sunt gestionați prin `SerializableMixin`. Se salvează doar atributele declarate în `SERIALISABLE = [...]`. Nodurile neregistrate în sistem sunt omise automat în timpul încărcării.

---

## 💾 `data/` – Date Procesate și Array-uri NumPy

Când un flux este executat, nodurile își pot stoca rezultatele în fișiere `.npy` din interiorul acestui folder.

- Numele fișierului coincide de obicei cu `id`-ul nodului sau cu referințele interne.
- Array-urile sunt stocate în format binar NumPy, **păstrând strict dimensionalitatea originală** (1D, 2D, 3D, etc.). Motorul nu aplică niciodată `flatten()`.
- În `diagram.json`, datele sunt referențiate cu prefixul `__npy__:`:
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- Dacă un nod nu produce date sau este configurat să nu le persiste, fișierul corespunzător poate fi omis.

??? note "Compatibilitate Externă"
    Fișierele `.npy` sunt universale în ecosistemul Python. Le puteți citi în afara FloWorks cu:
    ```python
    import numpy as np
    date = np.load("data/node_1.npy")
    print(date.shape)
    ```

---

## 🏷️ `metadata.json` (Opțional)

Conține informații descriptive care nu afectează execuția, ideal pentru trasabilitate și gestionarea proiectelor:

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "Analiza vibrațiilor în motor trifazat",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["inginerie", "vibrații", "FFT", "multicanal"]
}
```

---

## 🔄 Procesul de Salvare și Încărcare

FloWorks implementează un mecanism robust pentru a garanta integritatea datelor:

1. **Salvarea:**
   - Graful este parcurs și nodurile sunt serializate prin `SerializableMixin`.
   - Array-urile sunt extrase în `data/` și referențiate în JSON cu `__npy__:`.
   - Totul este ambalat într-un ZIP cu extensia `.sflow`.
2. **Încărcarea Sigură:**
   - Se creează o **copie de rezervă temporară în memorie** a diagramei curente.
   - Noul `.sflow` este extras și parsat.
   - Dacă apare orice eroare (JSON invalid, noduri lipsă, corupere `.npy`), **backup-ul este restaurat automat** fără pierderea muncii.
3. **ScriptNode Special:**
   - Salvează `script`, `params`, `dynamic_inputs`, `dynamic_outputs`, `persist` și `python_path`.
   - La încărcare, recompilează codul, reconstruiește porturile dinamice și restaurează automat starea `persist`.
4. **StickyNotes și DPI:**
   - Coordonatele și dimensiunile sunt normalizate la **96 dpi** la salvare.
   - La încărcare, se scalează la DPI-ul monitorului curent, garantând consistența vizuală între diferite rezoluții.

---

## 🛠️ Utilizare Externă și Automatizare

Formatul `.sflow` este proiectat să fie transparent și programatic. Îl puteți citi sau genera din scripturi externe:

=== "🐍 Python (Citire)"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("mi-flujo.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        date_n1 = np.load(z.open("data/node_1.npy"))
        print(f"Noduri: {len(graph['nodes'])}")
        print(f"Date: {date_n1.shape}")
    ```

=== "📤 Python (Creare de Bază)"
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

## 🔮 Compatibilitate și Extensibilitate Viitoare

Formatul `.sflow` urmează principiile **designului extensibil și retrocompatibil**:

- ✅ **Secțiuni noi:** Versiunile viitoare pot adăuga foldere precum `thumbnails/`, `logs/` sau `plugins/` fără a sparge încărcătoarele vechi.
- ✅ **Câmpuri opționale:** Parserul ignoră cheile necunoscute din `diagram.json`, permițând adăugarea de metadate experimentale.
- ✅ **Versionare:** Câmpul `floworks_version` din `metadata.json` permite aplicației să aplice migrații automate dacă formatul evoluează.

!!! warning "Regula de Aur"
    Nu modificați niciodată manual `diagram.json` cât timp aplicația este deschisă. Sistemul depinde de consistența dintre topologie, array-uri și starea vizualizării. Folosiți întotdeauna fluxurile native de salvare/încărcare.

---

## 📚 Resurse Conexe
- [🗺️ Harta Codului și Arhitecturii](architecture-ii.md) → Cum gestionează `file_io.py` și `SerializableMixin` formatul.
- [📦 Ghid de Build Portabil](guia-ejecutable-portable.md) → Ambalare și căi sigure pentru resurse.
- [🧩 Referință Tehnică a Nodurilor](node-reference.md) → Contracte de serializare pe tip de nod.
