---
title: Dateiformat .sflow
description: Technische Spezifikation, interne Struktur und Nutzungsleitfaden des Austauschstandards von FloWorks
---

# 📄 Dateiformat `.sflow`

Das Format `.sflow` ist der native Austausch- und Persistenzstandard von **FloWorks**. Es ermöglicht, einen vollständigen Arbeitsablauf in einer einzigen Datei zu verpacken, die die Graphtopologie, Knotenparameter, verarbeitete Daten und Haftnotizen enthält, wodurch das Teilen, Archivieren oder die deterministische Reproduktion von Experimenten erleichtert wird.

---

## 📦 Was ist eine `.sflow`-Datei?

Eine `.sflow`-Datei ist im Wesentlichen eine **umbenannte ZIP-Datei**. Indem Sie ihre Erweiterung in `.zip` ändern, können Sie ihren Inhalt mit jedem Dateimanager oder Kommandozeilentool inspizieren.

Ihre minimale interne Struktur besteht aus:

| Komponente | Beschreibung |
|------------|-------------|
| `diagram.json` | Hauptmanifest: definiert Knoten, Verbindungen, Ansicht, Haftnotizen und Serialisierungsmetadaten. |
| `data/` | Ordner mit den Daten jedes Knotens im Format `.npy` (binäre NumPy-Arrays). |
| `metadata.json` *(optional)* | Ergänzende Informationen: Autor, FloWorks-Version, Beschreibung und Tags. |

=== "🌳 Visuelle Struktur"
    ```text
    mi-flujo.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (optional, für ScriptNode persist)
    ```

---

## 🧩 `diagram.json` – Das Herz des Flusses

Diese JSON-Datei beschreibt die vollständige Topologie, die Position der Elemente auf der Zeichenfläche und den Zustand der Ansicht zum Zeitpunkt des Speicherns.

### Minimales Beispiel
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
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "Revisar umbral", "user_modified": true }
  ]
}
```

### Hauptfelder
| Feld | Typ | Beschreibung |
|-------|------|-------------|
| `nodes` | `Array` | Liste von Objekten `{id, type, pos, params}`. `type` muss mit `node_registry.py` bereinstimmen. |
| `connections` | `Array` | Liste von Verbindungen `{from, to, from_port, to_port}`. Ports sind Zeichenketten, keine Indizes. |
| `viewport` | `Object` | `(x, y, scale)` zum Wiederherstellen der exakten Position und des Zooms der Zeichenfläche. |
| `stickers` | `Array` | Serialisierte Haftnotizen mit auf 96 dpi normalisierten Koordinaten. |

!!! tip "Knotenserialisierung"
    Die spezifischen Parameter jedes Knotens werden über `SerializableMixin` verwaltet. Nur die in `SERIALISABLE = [...]` deklarierten Attribute werden gespeichert. Nicht im System registrierte Knoten werden beim Laden automatisch übersprungen.

---

## 💾 `data/` – Verarbeitete Daten und NumPy-Arrays

Wenn ein Fluss ausgeführt wird, können die Knoten ihre Ergebnisse in `.npy`-Dateien innerhalb dieses Ordners speichern.

- Der Dateiname stimmt normalerweise mit der `id` des Knotens oder internen Referenzen überein.
- Die Arrays werden im binären NumPy-Format gespeichert, **wobei die ursprüngliche Dimensionalität streng bewahrt** wird (1D, 2D, 3D usw.). Die Engine wendet niemals `flatten()` an.
- In `diagram.json` werden die Daten mit dem Präfix `__npy__:` referenziert:
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- Wenn ein Knoten keine Daten produziert oder so konfiguriert ist, dass er sie nicht persistiert, kann die entsprechende Datei weggelassen werden.

??? note "Externe Kompatibilität"
    `.npy`-Dateien sind im Python-Ökosystem universell. Sie können außerhalb von FloWorks gelesen werden mit:
    ```python
    import numpy as np
    datos = np.load("data/node_1.npy")
    print(datos.shape)
    ```

---

## 🏷️ `metadata.json` (Optional)

Enthält beschreibende Informationen, die die Ausführung nicht beeinflussen, ideal für Rückverfolgbarkeit und Projektmanagement:

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "Análisis de vibraciones en motor trifásico",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["ingeniería", "vibraciones", "FFT", "multicanal"]
}
```

---

## 🔄 Speicher- und Ladeprozess

FloWorks implementiert einen robusten Mechanismus zur Gewährleistung der Datenintegrität:

1. **Speichern:**
   - Der Graph wird durchlaufen und Knoten werden über `SerializableMixin` serialisiert.
   - Arrays werden nach `data/` extrahiert und in JSON mit `__npy__:` referenziert.
   - Alles wird in ein ZIP mit der Erweiterung `.sflow` verpackt.
2. **Sicheres Laden:**
   - Es wird eine **temporäre Sicherung im Speicher** des aktuellen Diagramms erstellt.
   - Die neue `.sflow` wird extrahiert und geparst.
   - Tritt ein Fehler auf (ungültiges JSON, fehlende Knoten, beschädigte `.npy`), **wird die Sicherung automatisch wiederhergestellt** ohne Arbeitsverlust.
3. **Spezieller ScriptNode:**
   - Speichert `script`, `params`, `dynamic_inputs`, `dynamic_outputs`, `persist` und `python_path`.
   - Beim Laden wird der Code neu kompiliert, die dynamischen Ports neu aufgebaut und der `persist`-Zustand automatisch wiederhergestellt.
4. **StickyNotes und DPI:**
   - Koordinaten und Größen werden beim Speichern auf **96 dpi** normalisiert.
   - Beim Laden werden sie auf die DPI des aktuellen Monitors skaliert, was visuelle Konsistenz zwischen verschiedenen Auflösungen gewährleistet.

---

## 🛠️ Externe Nutzung und Automatisierung

Das Format `.sflow` ist so konzipiert, dass es transparent und programmatisch ist. Sie können es aus externen Skripten lesen oder generieren:

=== "🐍 Python (Lesen)"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("mi-flujo.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        datos_n1 = np.load(z.open("data/node_1.npy"))
        print(f"Nodos: {len(graph['nodes'])}")
        print(f"Datos: {datos_n1.shape}")
    ```

=== "📤 Python (Grundlegende Erstellung)"
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

## 🔮 Kompatibilität und zukünftige Erweiterbarkeit

Das Format `.sflow` folgt den Prinzipien des **erweiterbaren und abwärtskompatiblen Designs**:

- ✅ **Neue Abschnitte:** Zukünftige Versionen können Ordner wie `thumbnails/`, `logs/` oder `plugins/` hinzufügen, ohne alte Loader zu brechen.
- ✅ **Optionale Felder:** Der Parser ignoriert unbekannte Schlüssel in `diagram.json`, was das Hinzufügen experimenteller Metadaten ermöglicht.
- ✅ **Versionierung:** Das Feld `floworks_version` in `metadata.json` erlaubt der Anwendung, automatische Migrationen anzuwenden, wenn sich das Format weiterentwickelt.

!!! warning "Goldene Regel"
    Ändern Sie `diagram.json` niemals manuell, während die Anwendung geöffnet ist. Das System hängt von der Konsistenz zwischen Topologie, Arrays und Ansichtszustand ab. Verwenden Sie immer die nativen Speicher-/Ladeflüsse.

---

## 📚 Verwandte Ressourcen
- [🗺️ Code-Karte und Architektur](architecture-ii.md) → Wie `file_io.py` und `SerializableMixin` das Format verwalten.
- [📦 Portable Build Leitfaden](guia-ejecutable-portable.md) → Paketierung und sichere Pfade für Ressourcen.
- [🧩 Knotenreferenz](node-reference.md) → Serialisierungsverträge pro Knotentyp.
