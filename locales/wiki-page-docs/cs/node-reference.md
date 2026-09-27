---
title: Technická reference uzlů
description: Aktualizovaný katalog, kontrakty rozšíření a pokročilé schopnosti systému uzlů ve FloWorks
---

# 🧩 Technická reference uzlů

FloWorks nezávisí na statickém katalogu. Používá **systém dynamické registrace** založený na jasných kontraktech. To umožňuje rozšiřovat platformu bez zásahu do topologického enginu. Níže je uveden implementovaný katalog, reálné technické schopnosti a protokol pro jeho bezpečné rozšíření.

---

## 📂 Kategorie jádra

=== "📦 Pohled vrstevně"
    <div class="grid cards" markdown>

    - **📥 Zdroje/Vstup**
      Generují nebo zachytávají počáteční signály. Podporují integrovanou simulaci, reálný hardware (VISA/SCPI) a multikanálový režim.
    - **⚙️ Zpracování**
      Transformují, kombinují nebo analyzují data. Zachovávají dimenzionalitu a automaticky interpolují, když je to nutné.
    - **🔀 Řízení/Tok**
      Bifurkují, iterují nebo podmiňují Spuštění. Zahrnují nativní podporu pro aktivační signály.
    - **🐍 Scripting/Pokročilé**
      Spouštějí dynamický Python kód s paramterickými porty (`# @param`), dynamickými porty (`# @input`/`# @output`) a persistencí stavu (`persist`).
    - **🔌 Hardware/Instrumentace**
      Rozhraní pro osciloskopy, LCR měřiče a generátory.
    - **📤 Výstup/Export**
      Vizualizují, exportují nebo archivují výsledky. Podporují vizuální témata, uživatelské profily a profesionální formáty (PNG/PDF/SVG).

    </div>

---

## 📋 Implementovaný technický katalog

| Uzel | Typ | Hlavní odpovědnost | Klíčové vlastnosti |
|------|-----|-------------------|-------------------|
| `SumNode` | Zpracování | Aritmetický operátor (+, -, *, /) pro dva vstupy. | Automaticky interpoluje signály různého rozlišení (FFTs). Zachovává dimenzionalitu. |
| `RhombusNode` | Řízení | Podmíněné větvení (Ano/Ne). | Dva výstupní porty. Vyhodnocuje podmínku podle prahu nebo booleovské logiky. |
| `TriggerNode` | Řízení | Iterátor/akumulátor s externí aktivací. | Přijímá `(x, y, "trigger")`. Akumuluje až N iterací a emituje výsledek složený/průměrovaný. |
| `ScriptNode` | Pokročilé | Integrované Python skriptovací prostředí. | QScintilla, automatické doplňování, `# @param`, dynamické porty, `persist`, šablony, konzole chyb, externí interpret s timeoutem. |
| `OscilloscopeNode` | Hardware | Zachytávání z osciloskopů (SDS) nebo LCR měřičů. | Simulační režim, integrovaný dialog firewallu, **podpora multikanálu** (`out_primary`, `out_secondary`), menu „Zobrazit kanál“. |
| `GeneratorNode` | Zdroj | Odesílání signálů do generátorů (SDG) nebo simulace výstupů. | Konfigurace modulace/sweep, integrovaný simulační dialog. |
| `GraphExporterNode` | Výstup | Profesionální exportér grafů. | Konfigurace dvojklikem, vlastní osy, témata, uložené profily atd. |

---

## 🔍 ScriptNode: Základní schopnosti

> **🐍 Integrované skriptovací prostředí**
>
> - **Integrovaný editor kódu:** Základní zvýrazňování syntaxe, číslování řádků a sbalování kódu.
> - **Panel dynamických parametrů:** Direktivy `# @param NÁZEV : typ = hodnota` injektují editovatelné ovládací prvky (spinbox, textové pole atd.) do postranního panelu.
> - **Dynamické porty:** `# @input název` a `# @output název` vytvářejí porty v reálném čase. Skript přijímá slovník `inputs` a vrací `outputs`.
> - **Persistence stavu:** Globální slovník `persist`, který uchovává hodnoty mezi Spuštěními.
> - **Šablony a Import/Export:** Rozbalovací menu se základními skripty. Uživatel může ukládat své skripty do `nodes/script_node/templates/` nebo importovat/exportovat externí `.py` soubory.
> - **Integrovaná konzole chyb:** Zobrazuje syntaktické/výkonnostní chyby s přesným označením řádky v editoru.
> - **Nápověda a i18n:** Kontextové tooltipy, tlačítko `?` s rychlou příručkou a všechny texty používají `tr()` pro překlad.
> - **Externí interpret s timeoutem:** Konfigurovatelná cesta (`# @python_path` nebo tlačítko „Procházet…"). Izolované Spuštění s časovým limitem a fallback na interní interpret.
> - **Úplná serializace:** Ukládá skript, parametry, dynamické porty a stav `persist`. Při načtení `.sflow` automaticky rekonstruuje porty a parametry.

---

## 📚 Související zdroje

- [📖 Mapa kódu a architektura](architecture-ii.md) → Odpovědnosti podle modulů a pracovní postupy.
- [🌐 Průvodce internacionalizací (i18n)](i18n.md) → Jak přidávat jazyky a spravovat klíče `tr()`.
- [🛠️ Přidat nový uzel (návod)](adding-a-new-node.md) → Krok za krokem s praktickými příklady.
- [📦 Průvodce sestavením a distribucí](build.md) → Balení PyInstaller, hooks a digitální podpisy.

---

💡 **Chybí v katalogu nějaký uzel?**
FloWorks je navržen jako rozšiřitelný. Pokud potřebujete uzel, který neexistuje, vytvořte ho podle kontraktu `BaseNode` a zaregistrujte ho. Komunita a budoucí marketplace budou ekosystém neustále rozšiřovat bez porušení kompatibility.
