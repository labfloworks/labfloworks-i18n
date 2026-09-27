---
title: Format pliku .sflow
description: Specyfikacja techniczna, struktura wewnętrzna i przewodnik użytkowania standardu wymiany FloWorks
---

# 📄 Format pliku `.sflow`

Format `.sflow` to natywny standard wymiany i trwałości **FloWorks**. Pozwala spakować kompletny przepływ pracy w jeden plik zawierający topologię grafu, parametry węzłów, przetworzone dane i karteczki samoprzylepne, ułatwiając udostępnianie, archiwizowanie lub deterministyczną reprodukcję eksperymentów.

---

## 📦 Czym jest plik `.sflow`?

Plik `.sflow` to w zasadzie **przemianowany plik ZIP**. Po zmianie rozszerzenia na `.zip` możesz przejrzeć jego zawartość w dowolnym menedżerze plików lub narzędziu wiersza poleceń.

Jego minimalna struktura wewnętrzna składa się z:

| Komponent | Opis |
|------------|-------------|
| `diagram.json` | Główny manifest: definiuje węzły, połączenia, widok, karteczki i metadane serializacji. |
| `data/` | Folder z danymi każdego węzła w formacie `.npy` (binarne tablice NumPy). |
| `metadata.json` *(opcjonalny)* | Informacje uzupełniające: autor, wersja FloWorks, opis i etykiety. |

=== " Struktura wizualna"
    ```text
    mi-flujo.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (opcjonalny, dla ScriptNode persist)
    ```

---

## 🧩 `diagram.json` – Serce przepływu

Ten plik JSON opisuje kompletną topologię, pozycję elementów na płótnie i stan widoku w momencie zapisu.

### Minimalny przykład
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
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "Sprawdź próg", "user_modified": true }
  ]
}
```

### Główne pola
| Pole | Typ | Opis |
|-------|------|-------------|
| `nodes` | `Array` | Lista obiektów `{id, type, pos, params}`. `type` musi odpowiadać `node_registry.py`. |
| `connections` | `Array` | Lista połączeń `{from, to, from_port, to_port}`. Porty to ciągi znaków, nie indeksy. |
| `viewport` | `Object` | `(x, y, scale)` do przywrócenia dokładnej pozycji i powiększenia płótna. |
| `stickers` | `Array` | Zserializowane karteczki ze współrzędnymi znormalizowanymi do 96 dpi. |

!!! tip "Serializacja węzłów"
    Specyficzne parametry każdego węzła są zarządzane przez `SerializableMixin`. Zapisywane są tylko atrybuty zadeklarowane w `SERIALISABLE = [...]`. Węzły niezarejestrowane w systemie są automatycznie pomijane podczas ładowania.

---

## 💾 `data/` – Przetworzone dane i tablice NumPy

Gdy przepływ jest wykonywany, węzły mogą przechowywać swoje wyniki w plikach `.npy` wewnątrz tego folderu.

- Nazwa pliku zazwyczaj odpowiada `id` węzła lub wewnętrznym referencjom.
- Tablice są przechowywane w binarnym formacie NumPy, **zachowując ściśle oryginalny wymiar** (1D, 2D, 3D itp.). Silnik nigdy nie stosuje `flatten()`.
- W `diagram.json` dane są referencjonowane z prefiksem `__npy__:`:
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- Jeśli węzeł nie produkuje danych lub jest skonfigurowany, aby ich nie utrwalać, odpowiedni plik może zostać pominięty.

??? note "Kompatybilność zewnętrzna"
    Pliki `.npy` są uniwersalne w ekosystemie Python. Możesz je odczytać poza FloWorks za pomocą:
    ```python
    import numpy as np
    dane = np.load("data/node_1.npy")
    print(dane.shape)
    ```

---

## 🏷️ `metadata.json` (Opcjonalny)

Zawiera informacje opisowe, które nie wpływają na wykonanie, idealne dla śledzenia i zarządzania projektami:

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "Analiza drgań w silniku trójfazowym",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["inżynieria", "drgania", "FFT", "wielokanałowy"]
}
```

---

## 🔄 Proces zapisywania i ładowania

FloWorks implementuje solidny mechanizm gwarantujący integralność danych:

1. **Zapisywanie:**
   - Graf jest przeszukiwany, a węzły są serializowane przez `SerializableMixin`.
   - Tablice są wyodrębniane do `data/` i referencjonowane w JSON za pomocą `__npy__:`.
   - Wszystko jest pakowane do ZIP z rozszerzeniem `.sflow`.
2. **Bezpieczne ładowanie:**
   - Tworzona jest **tymczasowa kopia zapasowa w pamięci** aktualnego diagramu.
   - Nowy `.sflow` jest wyodrębniany i parsowany.
   - Jeśli wystąpi jakikolwiek błąd (nieprawidłowy JSON, brakujące węzły, uszkodzenie `.npy`), **kopia zapasowa jest automatycznie przywracana** bez utraty pracy.
3. **Specjalny ScriptNode:**
   - Zapisuje `script`, `params`, `dynamic_inputs`, `dynamic_outputs`, `persist` i `python_path`.
   - Podczas ładowania kod jest ponownie kompilowany, dynamiczne porty są odbudowywane, a stan `persist` jest automatycznie przywracany.
4. **StickyNotes i DPI:**
   - Współrzędne i rozmiary są normalizowane do **96 dpi** podczas zapisywania.
   - Podczas ładowania są skalowane do DPI aktualnego monitora, zapewniając wizualną spójność między różnymi rozdzielczościami.

---

## 🛠️ Zewnętrzne użycie i automatyzacja

Format `.sflow` jest zaprojektowany tak, aby być przejrzystym i programowalnym. Możesz go odczytywać lub generować z zewnętrznych skryptów:

=== "🐍 Python (Odczyt)"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("mi-flujo.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        dane_n1 = np.load(z.open("data/node_1.npy"))
        print(f"Węzły: {len(graph['nodes'])}")
        print(f"Dane: {dane_n1.shape}")
    ```

=== "📤 Python (Podstawowe tworzenie)"
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

## 🔮 Kompatybilność i przyszła rozszerzalność

Format `.sflow` podąża za zasadami **rozszerzalnego i wstecznie kompatybilnego projektu**:

- ✅ **Nowe sekcje:** Przyszłe wersje mogą dodawać foldery takie jak `thumbnails/`, `logs/` lub `plugins/` bez psucia starych ładowarek.
- ✅ **Pola opcjonalne:** Parser ignoruje nieznane klucze w `diagram.json`, umożliwiając dodawanie eksperymentalnych metadanych.
- ✅ **Wersjonowanie:** Pole `floworks_version` w `metadata.json` pozwala aplikacji stosować automatyczne migracje, jeśli format ewoluuje.

!!! warning "Złota zasada"
    Nigdy nie modyfikuj ręcznie `diagram.json`, gdy aplikacja jest otwarta. System zależy od spójności między topologią, tablicami a stanem widoku. Zawsze używaj natywnych przepływów zapisywania/ładowania.

---

## 📚 Powiązane zasoby
- [🗺️ Mapa kodu i architektury](architecture-ii.md) → Jak `file_io.py` i `SerializableMixin` zarządzają formatem.
- [📦 Przewodnik po przenośnym buildzie](guia-ejecutable-portable.md) → Pakowanie i bezpieczne ścieżki dla zasobów.
- [🧩 Techniczna referencja węzłów](node-reference.md) → Kontrakty serializacji według typu węzła.
