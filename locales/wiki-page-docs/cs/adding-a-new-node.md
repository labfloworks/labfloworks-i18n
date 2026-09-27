---
title: Průvodce přidáním nového uzlu do FloWorks
description: Návod krok za krokem pro vytvoření, registraci a integraci vlastních uzlů do výpočetního jádra FloWorks.
---

# 📘 Průvodce pro vývojáře: Jak přidat nový uzel do FloWorks

Tato příručka popisuje kompletní proces vytvoření nového typu uzlu ve FloWorks a zajišťuje jeho správnou integraci s výpočetním jádrem, uživatelským rozhraním, vizuálními tématy a systémem internacionalizace.

---

## 📋 Obsah
- [📘 Průvodce pro vývojáře: Jak přidat nový uzel do FloWorks](#-průvodce-pro-vývojáře-jak-přidat-nový-uzel-do-floworks)
  - [📋 Obsah](#-obsah)
  - [1. Úvod do architektury](#1-úvod-do-architektury)
  - [2. Použití šablony `template_node.py`](#2-použití-šablony-template_nodepy)
  - [3. Krok za krokem: Vytvoření vlastního uzlu](#3-krok-za-krokem-vytvoření-vlastního-uzlu)
    - [3.1. Kopírování a přejmenování šablony](#31-kopírování-a-přejmenování-šablony)
    - [3.2. Definice portů a štítků](#32-definice-portů-a-štítků)
    - [3.3. Implementace zpracovací logiky](#33-implementace-zpracovací-logiky)
    - [3.4. Přizpůsobení vzhledu (volitelné)](#34-přizpůsobení-vzhledu-volitelné)
    - [3.5. Přidání konfigurovatelných parametrů (volitelné)](#35-přidání-konfigurovatelných-parametrů-volitelné)
    - [3.6. Serializace uzlu (ukládání / načítání konfigurací)](#36-serializace-uzlu-ukládání--načítání-konfigurací)
  - [4. Integrace do systému](#4-integrace-do-systému)
  - [5. Internacionalizace (i18n)](#5-internacionalizace-i18n)
  - [6. Vizuální témata](#6-vizuální-témata)
  - [7. Kontrolní seznam a řešení problémů](#7-kontrolní-seznam-a-řešení-problémů)
    - [✅ Kontrolní seznam](#-kontrolní-seznam)
    - [🐛 Časté problémy](#-časté-problémy)
  - [8. Závěr](#8-závěr)

---

## 1. Úvod do architektury

FloWorks je postaveno na PySide6 a využívá model propojitelných uzlů reprezentujících tok zpracování signálů.

---

## 2. Použití šablony `template_node.py`

Pro usnadnění tvorby nových uzlů je k dispozici soubor `nodes/template_node.py`. Tato šablona obsahuje:
- Plnou podporu internacionalizace (připojení k `languageChanged`, metoda `update_language`).
- Plnou podporu témat (metoda `update_theme`).
- Integrovanou nápovědu ve formátu HTML se třemi sekcemi.
- Správu více konfigurovatelných vstupních/výstupních portů.
- Více výstupů pomocí `get_output_for_port`.
- Vizualizaci v grafu prostřednictvím `get_display_signal`.
- Přeložitelnou kontextovou nabídku.

Při vývoji nového uzlu se vždy doporučuje vycházet z této šablony.

---

## 3. Krok za krokem: Vytvoření vlastního uzlu

### 3.1. Kopírování a přejmenování šablony
1. Zkopírujte `nodes/template_node.py` pod názvem vašeho nového uzlu, např. `nodes/mi_nodo.py`.
2. Přejmenujte třídu z `TemplateNode` na něco popisného, např. `MiNodoNode`.
3. Upravte importy, pokud je to nutné.

### 3.2. Definice portů a štítků
!!! warning "Důležité: Shoda názvů"
    Názvy portů v `PORTS`, `PORT_LABELS` a klíče slovníku vráceného `execute_program` musí být **přesně stejné** (včetně velkých/malých písmen). Šablona nyní obsahuje mapování aliasů (`'data_in'` → první levý port) pro větší robustnost.

Upravte slovník `PORTS` v horní části souboru. Každý záznam má formát:
```python
"nazev_portu": ("strana", zlomek)
```
- **Možné strany:** `"left"`, `"right"`, `"top"`, `"bottom"`.
- **Zlomek:** hodnota mezi `0.0` a `1.0` udávající pozici podél strany.

**Příklad uzlu s jedním vstupem a dvěma výstupy:**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
Slovník `PORT_LABELS` obsahuje text, který se zobrazí vedle každého portu. Doporučuje se používat klíče překladů místo pevného textu (viz sekce Internacionalizace).

### 3.3. Implementace zpracovací logiky
Klíčovou metodou je `execute_program(self, input_data)`. Tuto metodu vyvolá výpočetní jádro, když uzel obdrží data.

**`input_data` může být:**
- `None`, pokud není žádný vstup.
- N-tice `(x, y)` pro časové signály.
- 1D pole.
- Slovník `{nazev_portu: data}` u uzlů s více vstupy.

**Návratová hodnota:**
- Pro uzly s jediným výstupem vraťte data přímo (např. n-tici `(x, y)`).
- Pro uzly s více výstupy vraťte slovník, jehož klíče odpovídají názvům výstupních portů definovaných v `PORTS`.

```python
def execute_program(self, input_data):
    # Zpracuj input_data a vygeneruj výsledky
    vysledek_magnitude = (freq, mag)
    vysledek_faze = (freq, phase)
    return {
        "magnitude": vysledek_magnitude,
        "phase": vysledek_faze
    }
```

!!! tip "Poznámka k obecným názvům portů"
    Výpočetní jádro může občas předat slovník s klíči jako `'data_in'` místo skutečného názvu portu (zejména pokud uživatel neklikl přesně na kruh). Šablona již obsahuje kód pro zpracování tohoto případu:
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    To zabraňuje selhání uzlu kvůli nepřesnému připojení.

Šablona již obsahuje okomentovaný příklad. Dále implementuje `get_output_for_port(self, port_name)`, aby jádro mohlo směrovat každý výstup:
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. Přizpůsobení vzhledu (volitelné)
Metoda `paint()` vykresluje pozadí, nadpis, stav a jakýkoli další text. Můžete upravit:
- Barvy (automaticky se aktualizují pomocí `update_theme`).
- Text stavu (pomocí atributu `self._status`).
- Souhrnné informace (např. špička magnitudy).

Šablona obsahuje základní příklad.

### 3.5. Přidání konfigurovatelných parametrů (volitelné)
Pokud váš uzel vyžaduje parametry nastavitelné uživatelem (např. velikost okna, mezní frekvence), můžete:
1. Přidat atributy v `__init__` (např. `self.window_size = 512`).
2. Vytvořit konfigurační dialog (dědí z `QDialog`).
3. Připojit dialog v `open_config_dialog()` (metoda již přítomna v šabloně).
4. Aktualizovat parametry z dialogu a zavolat `self.update()`.

### 3.6. Serializace uzlu (ukládání / naítání konfigurací)
Aby uzel mohl ukládat a obnovovat své parametry při kopírování/vkládání, vracení/znovu provedení, nebo při použití příkazů Uložit/Otevřít v nabídce Soubor, musí dědit z mixin serializace a deklarovat své atributy.

1. Importujte mixin do svého souboru:
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. Změňte dědičnost třídy tak, aby zahrnovala mixin před `QGraphicsObject`:
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
    ```
3. Definujte seznam `SERIALISABLE` na úrovni třídy s názvy atributů, které chcete uchovat. Podporuje pouze jednoduché typy (`int`, `float`, `str`, `bool`), seznamy, slovníky nebo pole NumPy (ta se automaticky ukládají jako soubory `.npy` uvnitř `.sflow`).
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frecuencia', 'amplitud', 'configuracion']
    ```
4. Ujistěte se, že jsou tyto atributy inicializovány v `__init__`:
    ```python
    self.frecuencia = 1000.0
    self.amplitud = 1.0
    self.configuracion = {'tipo': 'seno', 'fase': 0}
    ```

S tímto nemusíte psát metody `serialize`/`deserialize`; mixin se o ukládání a obnovu hodnot postará automaticky.

Pokud váš uzel vyžaduje dodatečnou logiku při načítání (např. znovu připojení hardwarového zařízení), můžete přepsat `deserialize` s voláním nadřazené metody:
```python
def deserialize(self, data):
    super().deserialize(data)   # obnoví atributy ze SERIALISABLE
    self._iniciar_dispositivo()
```

---

## 4. Integrace do systému

Jakmile vytvoříte soubor uzlu, stačí jej vložit do složky `nodes`, aby se objevil v rozhraní a fungoval se zbytkem systému.

---

## 5. Internacionalizace (i18n)

Všechny viditelné texty musí být přeložitelné prostřednictvím `tr("klic", default="...")`. Šablona to již implementuje. Musíte přidat odpovídající klíče do JSON souborů ve složce `locales/`.

**Doporučená struktura:**
```json
{
   "nodes": {
     "mi_nodo": {
       "title": "Mi Nodo",
       "tooltip": "Vyskakovací popis",
       "ports": {
         "input": "Vstup",
         "output1": "Výstup 1",
         "output2": "Výstup 2"
      },
       "status": {
         "no_data": "Žádná data",
         "ready": "Připraven"
      },
       "menu": {
         "show_output": "Zobrazit výstup",
         "configure": "Nastavit..."
      },
       "help_title": "Nápověda - Mi Nodo",
       "help_html": "<h3>🎛️ Filter Node</h3>
<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_mi_nodo": "Mi Nodo"
  }
}
```

Nápověda HTML sleduje formát tří sekcí společný všem uzlům (specifický popis + "Jak přemýšlet o systému" + "Zkratky a triky"). Šablona již obsahuje strukturu v `get_help_text()`.

---

## 6. Vizuální témata

Metoda `update_theme(self, theme)` obdrží slovník s barvami definovanými aktuálním tématem. Šablona automaticky aktualizuje:
- Pozadí uzlu (`node_normal_bg`)
- Okraj (`node_selected_border`)
- Barvu nadpisu a textu (`node_normal_text`)
- Barvy portů (`port_circle`, `port_outline`, `port_inline`, `port_text`)

Ujistěte se, že v `MainWindow` (nebo `ThemeUpdater`) se volá `node.update_theme()` pro každý uzel při změně tématu.

---

## 7. Kontrolní seznam a řešení problémů

### ✅ Kontrolní seznam
- [ ] Uzel se správně vytváří z panelu nástrojů.
- [ ] Porty se zobrazují na očekávaných pozicích a jsou detekovatelné pro připojení (`Ctrl+klik`).
- [ ] Při přijetí vstupních dat je voláno `execute_program` a signál je zpracován.
- [ ] Výstupy se správně propagují do připojených uzlů.
- [ ] Kontextová nabídka umožňuje změnit kanál vizualizace (pokud existuje více výstupů).
- [ ] Po kliknutí na uzel se vybraný signál vykreslí v grafu.
- [ ] Dvojklik otevře nápovědu ve správném formátu.
- [ ] Jazyk se správně mění (texty nadpisů, portů, nabídek).
- [ ] Téma se správně aplikuje (barvy uzlu a portů).
- [ ] Kopírování/vkládání funguje bez chyb.

!!! tip "Přesné připojení portů"
    Při propojování uzlů se ujistěte, že klikáte přesně na kruh cílového portu. Pokud kliknete na tělo uzlu, systém použije obecný název (`'data_in'`). Šablona tyto názvy nyní toleruje, ale je dobrým zvykem připojovat se přímo ke kruhu pro zajištění správného směrování více výstupů.

### 🐛 Časté problémy

| Příznak | Možná příčina | Řešení |
|---------|---------------|----------|
| Spojovací šipka se neukotví k portu. | Kruh portu nemá `setData(0, port_name)` nebo `get_port_scene_pos` není implementováno. | Ověřte, že v `_create_ports` je `circle.setData(0, port_name)` a že `get_port_scene_pos` používá tento název. |
| Výstupy nedocházejí do připojených uzlů. | `execute_program` nevrací slovník (pro více výstupů) nebo `get_output_for_port` není implementováno. | Zajistěte, aby `execute_program` vracel `{nazev_portu: data}` a aby `get_output_for_port` vracel odpovídající hodnotu. |
| Po kliknutí na uzel se nic nevykreslí. | `get_display_signal` nevrací platnou n-tici `(x, y)` nebo `display_channel` neodpovídá existujícímu výstupu. | Zkontrolujte, že `get_display_signal` používá vybraný kanál a že data jsou pole NumPy. |
| Texty se neaktualizují při změně jazyka. | Nepřipojil se signál `languageChanged` nebo `update_language` neaktualizuje prvky. | Ověřte připojení v `__init__`: `language_manager.languageChanged.connect(self.update_language)`. |
| Téma se nepoužije. | Nezavolá se `update_theme` při vytvoření uzlu nebo při změně tématu. | V `MainWindow` po vytvoření uzlu zavolejte `node.update_theme(self.theme_manager.current_theme())`. |
| Šipka míří do středu uzlu. | Kliklo se na tělo místo kruhu, nebo název neodpovdá `PORTS`. | Klikněte přímo na kruh. Ověřte, že `get_port_scene_pos` obsahuje mapování aliasů. |
| `NameError: name 'self' is not defined` při importu. | Atributy instance byly deklarovány mimo `__init__`. | Všechny atributy jako `self.muj_parametr` musí být definovány uvnitř `__init__`. |
| Parametry se ztrácejí při kopírování/otevření `.sflow`. | Uzel nedědí z `SerializableMixin` nebo není definováno `SERIALISABLE`. | Implementujte krok 3.6 této příručky. |

---

## 8. Závěr

Podle této příručky a s využitím šablony `template_node.py` budete moci efektivně a soudrně se zbytkem systému přidávat nové uzly do FloWorks. Vždy si pamatujte zachovat kompatibilitu s i18n a tématy pro profesionální uživatelský zážitek.

Přispějte svými vlastními uzly!
