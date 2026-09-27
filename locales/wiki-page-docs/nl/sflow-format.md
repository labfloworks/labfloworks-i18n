---
title: .sflow Bestandsformaat
description: Technische specificatie, interne structuur en gebruikshandleiding van het FloWorks uitwisselingsstandaard
---

# 📄 `.sflow` Bestandsformaat

Het `.sflow` formaat is de native uitwisselings- en persistentiestandaard van **FloWorks**. Het maakt het mogelijk om een volledige werkstroom te verpakken in een enkel bestand dat de graaftopologie, knooppuntparameters, verwerkte gegevens en plaknotities bevat, waardoor delen, archiveren of deterministisch reproduceren van experimenten wordt vergemakkelijkt.

---

## 📦 Wat is een `.sflow` bestand?

Een `.sflow` bestand is in wezen **een hernoemd ZIP-bestand**. Door de extensie te wijzigen in `.zip`, kunt u de inhoud ervan inspecteren met elke bestandsbeheerder of opdrachtregeltool.

De minimale interne structuur bestaat uit:

| Component | Beschrijving |
|------------|-------------|
| `diagram.json` | Hoofdmanifest: definieert knopen, verbindingen, weergave, plaknotities en serialisatiemetadata. |
| `data/` | Map met de gegevens van elke knoop in `.npy` formaat (NumPy binaire arrays). |
| `metadata.json` *(optioneel)* | Aanvullende informatie: auteur, FloWorks-versie, beschrijving en labels. |

=== "🌳 Visuele Structuur"
    ```text
    mi-flujo.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (optioneel, voor ScriptNode persist)
    ```

---

## 🧩 `diagram.json` – Het Hart van de Stroom

Dit JSON-bestand beschrijft de volledige topologie, de positie van elementen op het canvas en de staat van de weergave op het moment van opslaan.

### Minimaal Voorbeeld
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
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "Controleer drempel", "user_modified": true }
  ]
}
```

### Belangrijkste Velden
| Veld | Type | Beschrijving |
|-------|------|-------------|
| `nodes` | `Array` | Lijst van `{id, type, pos, params}` objecten. `type` moet overeenkomen met `node_registry.py`. |
| `connections` | `Array` | Lijst van verbindingen `{from, to, from_port, to_port}`. Poorten zijn strings, geen indexen. |
| `viewport` | `Object` | `(x, y, scale)` om de exacte positie en zoom van het canvas te herstellen. |
| `stickers` | `Array` | Geserialiseerde plaknotities met op 96 dpi genormaliseerde coördinaten. |

!!! tip "Serialisatie van Knopen"
    De specifieke parameters van elke knoop worden beheerd via `SerializableMixin`. Alleen attributen gedeclareerd in `SERIALISABLE = [...]` worden opgeslagen. Knopen die niet in het systeem zijn geregistreerd, worden automatisch overgeslagen tijdens het laden.

---

## 💾 `data/` – Verwerkte Gegevens en NumPy Arrays

Wanneer een stroom wordt uitgevoerd, kunnen knopen hun resultaten opslaan in `.npy` bestanden binnen deze map.

- De bestandsnaam komt meestal overeen met de `id` van de knoop of met interne referenties.
- Arrays worden opgeslagen in NumPy binair formaat, **waarbij de oorspronkelijke dimensionaliteit strikt behouden blijft** (1D, 2D, 3D, etc.). De motor past nooit `flatten()` toe.
- In `diagram.json` worden gegevens gerefereerd met het voorvoegsel `__npy__:`:
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- Als een knoop geen gegevens produceert of is geconfigureerd om deze niet te persisteren, kan het corresponderende bestand worden weggelaten.

??? note "Externe Compatibiliteit"
    `.npy` bestanden zijn universeel in het Python-ecosysteem. U kunt ze buiten FloWorks lezen met:
    ```python
    import numpy as np
    gegevens = np.load("data/node_1.npy")
    print(gegevens.shape)
    ```

---

## 🏷️ `metadata.json` (Optioneel)

Bevat beschrijvende informatie die de uitvoering niet beïnvloedt, ideaal voor traceerbaarheid en projectbeheer:

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "Trillingsanalyse in driefasenmotor",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["techniek", "trillingen", "FFT", "multikanaals"]
}
```

---

## 🔄 Proces van Opslaan en Laden

FloWorks implementeert een robuust mechanisme om de integriteit van gegevens te garanderen:

1. **Opslaan:**
   - De graaf wordt doorlopen en knopen worden geserialiseerd via `SerializableMixin`.
   - Arrays worden uitgepakt naar `data/` en gerefereerd in JSON met `__npy__:`.
   - Alles wordt verpakt in een ZIP met extensie `.sflow`.
2. **Veilig Laden:**
   - Er wordt een **tijdelijk geheugenbackup** van het huidige diagram gemaakt.
   - Het nieuwe `.sflow` wordt uitgepakt en geparseerd.
   - Als er een fout optreedt (ongeldige JSON, ontbrekende knopen, corruptie van `.npy`), **wordt de backup automatisch hersteld** zonder verlies van werk.
3. **Speciale ScriptNode:**
   - Slaat `script`, `params`, `dynamic_inputs`, `dynamic_outputs`, `persist` en `python_path` op.
   - Bij het laden wordt de code opnieuw gecompileerd, worden dynamische poorten herbouwd en wordt de `persist` staat automatisch hersteld.
4. **StickyNotes en DPI:**
   - Coördinaten en afmetingen worden genormaliseerd naar **96 dpi** bij het opslaan.
   - Bij het laden worden ze geschaald naar de DPI van het huidige beeldscherm, wat visuele consistentie tussen verschillende resoluties garandeert.

---

## 🛠️ Extern Gebruik en Automatisering

Het `.sflow` formaat is ontworpen om transparant en programmatisch te zijn. U kunt het lezen of genereren vanuit externe scripts:

=== "🐍 Python (Lezen)"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("mi-flujo.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        gegevens_n1 = np.load(z.open("data/node_1.npy"))
        print(f"Knopen: {len(graph['nodes'])}")
        print(f"Gegevens: {gegevens_n1.shape}")
    ```

=== "📤 Python (Basis Aanmaak)"
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

## 🔮 Compatibiliteit en Toekomstige Uitbreidbaarheid

Het `.sflow` formaat volgt de principes van **uitbreidbaar en achterwaarts compatibel ontwerp**:

- ✅ **Nieuwe secties:** Toekomstige versies kunnen mappen zoals `thumbnails/`, `logs/` of `plugins/` toevoegen zonder oude laders te breken.
- ✅ **Optionele velden:** De parser negeert onbekende sleutels in `diagram.json`, waardoor experimentele metadata kan worden toegevoegd.
- ✅ **Versionering:** Het veld `floworks_version` in `metadata.json` stelt de toepassing in staat automatische migraties toe te passen als het formaat evolueert.

!!! warning "Gouden Regel"
    Wijzig `diagram.json` nooit handmatig terwijl de toepassing open is. Het systeem is afhankelijk van consistentie tussen topologie, arrays en de staat van de weergave. Gebruik altijd de native opslaan/laden stromen.

---

## 📚 Gerelateerde Bronnen
- [🗺️ Kaart van Code en Architectuur](architecture-ii.md) → Hoe `file_io.py` en `SerializableMixin` het formaat beheren.
- [📦 Handleiding voor Draagbare Build](guia-ejecutable-portable.md) → Verpakking en veilige paden voor bronnen.
- [🧩 Technische Referentie van Knopen](node-reference.md) → Serialisatiecontracten per knooptype.
