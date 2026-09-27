---
title: Formát souboru .sflow
description: Technická specifikace, vnitřní struktura a příručka použití standardu výměny FloWorks
---

# 📄 Formát souboru `.sflow`

Formát `.sflow` je nativní standard výměny a persistence **FloWorks**. Umožňuje zabalit celý pracovní postup do jediného souboru, který obsahuje topologii grafu, parametry uzlů, zpracovaná data a lepicí poznámky, což usnadňuje sdílení, archivaci nebo deterministickou reprodukci experimentů.

---

## 📦 Co je soubor `.sflow`?

Soubor `.sflow` je v podstatě **přejmenovaný ZIP soubor**. Po změně přípony na `.zip` můžete prozkoumat jeho obsah libovolným správcem souborů nebo nástrojem příkazové řádky.

Jeho minimální vnitřní struktura se skládá z:

| Komponenta | Popis |
|------------|-------------|
| `diagram.json` | Hlavní manifest: definuje uzly, připojení, pohled, lepicí poznámky a metadata serializace. |
| `data/` | Složka s daty každého uzlu ve formátu `.npy` (binární pole NumPy). |
| `metadata.json` *(volitelné)* | Doplňkové informace: autor, verze FloWorks, popis a štítky. |

=== "🌳 Vizuální struktura"
    ```text
    mi-flujo.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (volitelné, pro ScriptNode persist)
    ```

---

## 🧩 `diagram.json` – Srdce toku

Tento JSON soubor popisuje úplnou topologii, pozici prvků na plátně a stav pohledu v okamžiku uložení.

### Minimální příklad
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
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "Zkontrolovat práh", "user_modified": true }
  ]
}
```

### Hlavní pole
| Pole | Typ | Popis |
|-------|------|-------------|
| `nodes` | `Array` | Seznam objektů `{id, type, pos, params}`. `type` musí odpovídat `node_registry.py`. |
| `connections` | `Array` | Seznam připojení `{from, to, from_port, to_port}`. Porty jsou řetězce, nikoli indexy. |
| `viewport` | `Object` | `(x, y, scale)` pro obnovení přesné pozice a zoomu plátna. |
| `stickers` | `Array` | Serializované lepicí poznámky se souřadnicemi normalizovanými na 96 dpi. |

!!! tip "Serializace uzlů"
    Specifické parametry každého uzlu se spravují prostřednictvím `SerializableMixin`. Ukládají se pouze atributy deklarované v `SERIALISABLE = [...]`. Uzly neregistrované v systému se při načítání automaticky vynechají.

---

## 💾 `data/` – Zpracovaná data a pole NumPy

Když je tok spuštěn, uzly mohou ukládat své výsledky do souborů `.npy` uvnitř této složky.

- Název souboru obvykle odpovídá `id` uzlu nebo interním referencím.
- Pole se ukládají v binárním formátu NumPy, **striktně zachovávají původní dimenzionalitu** (1D, 2D, 3D atd.). Motor nikdy nepoužije `flatten()`.
- V `diagram.json` se data odkazují s předponou `__npy__:`:
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- Pokud uzel nevytváří data nebo je má nakonfigurováno, aby je neukládal, může se odpovídající soubor vynechat.

??? note "Externí kompatibilita"
    Soubory `.npy` jsou univerzální v ekosystému Python. Můžete je číst mimo FloWorks pomocí:
    ```python
    import numpy as np
    data = np.load("data/node_1.npy")
    print(data.shape)
    ```

---

## 🏷️ `metadata.json` (Volitelné)

Obsahuje popisné informace, které neovlivňují spuštění, ideální pro trasovatelnost a správu projektů:

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "Analýza vibrací v trojfázovém motoru",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["inženýrství", "vibrace", "FFT", "multikanál"]
}
```

---

## 🔄 Proces ukládání a načítání

FloWorks implementuje robustní mechanismus pro zajištění integrity dat:

1. **Ukládání:**
   - Prochází se graf a uzly se serializují prostřednictvím `SerializableMixin`.
   - Pole se extrahují do `data/` a odkazují se v JSON pomocí `__npy__:`.
   - Vše se zabalí do ZIP s příponou `.sflow`.
2. **Bezpečné načítání:**
   - Vytvoří se **dočasná záloha v paměti** aktuálního diagramu.
   - Nový `.sflow` se extrahuje a parsuje.
   - Pokud dojde k jakékoli chybě (neplatný JSON, chybějící uzly, poškození `.npy`), **záloha se automaticky obnoví** bez ztráty práce.
3. **Speciální ScriptNode:**
   - Ukládá `script`, `params`, `dynamic_inputs`, `dynamic_outputs`, `persist` a `python_path`.
   - Při načítání kód znovu zkompiluje, zrekonstruuje dynamické porty a automaticky obnoví stav `persist`.
4. **StickyNotes a DPI:**
   - Souřadnice a rozměry se při ukládání normalizují na **96 dpi**.
   - Při načítání se škáluj na DPI aktuálního monitoru, což zaručuje vizuální konzistenci mezi různými rozlišeními.

---

## 🛠️ Externí použití a automatizace

Formát `.sflow` je navržen tak, aby byl transparentní a programovatelný. Můžete jej číst nebo generovat z externích skriptů:

=== "🐍 Python (Čtení)"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("mi-flujo.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        data_n1 = np.load(z.open("data/node_1.npy"))
        print(f"Uzly: {len(graph['nodes'])}")
        print(f"Data: {data_n1.shape}")
    ```

=== "📤 Python (Základní vytvoření)"
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

## 🔮 Kompatibilita a budoucí rozšiřitelnost

Formát `.sflow` následuje principy **rozšiřitelného a zpětně kompatibilního návrhu**:

- ✅ **Nové sekce:** Budoucí verze mohou přidávat složky jako `thumbnails/`, `logs/` nebo `plugins/` bez porušení starých načítačů.
- ✅ **Volitelná pole:** Parser ignoruje neznámé klíče v `diagram.json`, což umožňuje přidávat experimentální metadata.
- ✅ **Verzování:** Pole `floworks_version` v `metadata.json` umožňuje aplikaci aplikovat automatické migrace, pokud se formát vyvíjí.

!!! warning "Zlaté pravidlo"
    Nikdy neměňte `diagram.json` ručně, když je aplikace otevřená. Systém závisí na konzistenci mezi topologií, poli a stavem pohledu. Vždy používejte nativní tok ukládání/načítání.

---

## 📚 Související zdroje
- [🗺️ Mapa kódu a architektury](architecture-ii.md) → Jak `file_io.py` a `SerializableMixin` spravují formát.
- [📦 Průvodce přenosným buildem](guia-ejecutable-portable.md) → Balení a bezpečné cesty pro prostředky.
- [ Technická reference uzlů](node-reference.md) → Kontrakty serializace podle typu uzlu.
