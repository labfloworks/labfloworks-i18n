---
title: První kroky s FloWorks
description: Rychlý průvodce nastavením prostředí, spuštěním prvního toku a přístupem k přenosné verzi.
---

# 🚀 První kroky s FloWorks

Tato příručka vás provede od nuly až po spuštění vašeho prvního toku zpracování signálů. FloWorks je aplikace pro vývojové diagramy signálů postavená na Pythonu a PySide6, která podporuje reálný hardware (VISA/SCPI), integrovanou simulaci, pokročilé skriptování a změnu jazyka za běhu.

---

## 🌊 Váš první ukázkový tok

Vytvoříme jednoduchý tok: vygenerujeme sinusový signál a zobrazíme jej v reálném čase.

1. **Přidání uzlů**
   V horním panelu nástrojů vyberte `Zdroj` → vyberte `Pokročilý generátor signálů`. Poté z `Zpracování` → vyberte například `Spektrální`.
2. **Propojení**
   Stiskněte `Ctrl+Klik` na výstupní port (`pravý`) generátoru. Poté klikněte na vstupní port (`levý`) osciloskopu. Nebo jednoduše klikněte na výstupní port a přetáhněte (držte stisknuté) na vstupní port následujícího uzlu.
3. **Konfigurace (volitelné)**
   Klikněte na uzel, v levém bočním panelu se objeví editor s parametry vybraného uzlu pro úpravu provozních podmínek. V dolní části je grafické zobrazení, které vizuálně reprezentuje data generovaná nebo získaná uzly.
4. **Spuštění**
   Stiskněte `F5` nebo tlačítko ▶ na panelu nástrojů. Topologické jádro vypočítá pořadí spuštění, zpracuje data a vy uvidíte vlnu v grafickém panelu. **Konektor se zanimuje a ukáže aktivní tok!**

---

## 🧠 Porozumění portům: kategorie podle barev

Ve FloWorks každý port náleží do **funkční kategorie** identifikované barvou. Platná připojení se provádějí **vždy mezi porty stejné barvy**: výstup jedné kategorie se připojuje výhradně ke vstupu stejné kategorie. Navíc spojovací čára automaticky přejímá barvu propojených portů, což usnadňuje vizuální čtení.

| Typ | Barva | Účel | Typický příklad |
|------|-------|-----------|----------------|
| `control` | Bílá | Tok řízení / aktivace. | Spouštěcí signál k uzlu akvizice. |
| `exec` | Šedá | Vykonání operací nebo kroků. | Spuštění funkce nebo zpětného volání. |
| `data` | Zelená | Obecná data / číselné signály. | Výstup generátoru nebo senzoru. |
| `int` | Modrá | Celá čísla. | Index, velikost bufferu, ID. |
| `float` | Azurová | Čísla s plovoucí desetinnou čárkou. | Amplituda, frekvence, práh. |
| `string` | Fialová | Textové řetězce. | Název souboru, popisek. |
| `bool` | Růžová | Booleovské hodnoty (`True`/`False`). | Příznak stavu, povolení. |
| `array` | Tmavě modrá | Pole / vektory. | Vícekanálový signál, seznam vzorků. |
| `trigger` | Oranžová | Spouštěče / diskrétní události. | Synchronizační puls, hrana. |

**Zlaté pravidlo:**

- Připojují se pouze porty **přesně stejné barvy** (výstup ↔ vstup stejné kategorie).
- Systém zabraňuje neplatným připojením a při přetahování vizuálně zvýrazňuje kompatibilní porty.
- Spojovací čára přebírá barvu propojených portů; tak se každá trasa identifikuje na první pohled.

**Filozofie FloWorks:**
Datové porty **zachovávají dimenzionalitu** polí. Nikdy se nepoužije automatický flatten: pokud vstoupí matice, vystoupí matice, a zachovává se tak integrita vašich multidimenzionálních signálů.

![FloWorks](assets/tipos_de_puertos.PNG)

---

## 🖱️ Navigace po plátně (Canvas)

Ovládněte pracovní prostor těmito gesty:

| Akce | Jak na to |
|--------|--------------|
| **Zoom** | Kolečko myši nebo `Ctrl + kolečko` |
| **Posun (pan)** | Podržte `Mezerník` a táhněte, nebo použijte prostřední tlačítko myši |
| **Vybrat uzel** | Levý klik na uzel |
| **Vícenásobný výběr** | Přetáhněte obdélník levým tlačítkem, nebo `Ctrl + klik` na více uzlů |
| **Přesunout výběr** | Přetáhněte kterýkoli z vybraných uzlů |
| **Otevřít konfiguraci** | Dvojklik na uzel |

**Tip:** Levý panel se automaticky aktualizuje podle konfigurace vybraného uzlu, není třeba otevírat další okna.

---

## ⚡ Klávesové zkratky a pokročilé pohyby

Tyto zkratky promění běžného uživatele v **power usera**:

| Zkratka | Akce |
|-------|--------|
| `F5` | Spustit tok |
| `Ctrl + S` | Uložit projekt (`.sflow`) |
| `Ctrl + Klik` | Připojit uzly (klik na výstupní port → klik na vstupní port) |
| `Ctrl + C` / `Ctrl + V` | Kopírovat / vložit vybrané uzly |
| `Ctrl + Z` / `Ctrl + Y` | Zpět / vpřed |
| `Ctrl + Shift + L` | Automaticky uspořádat uzly na plátně |
| `Del` | Odstranit vybrané uzly |
| `Ctrl + A` | Vybrat vechny uzly |

**Pokročilé pohyby:**

- **Duplikovat tok:** vyberte skupinu uzlů, `Ctrl + C`, `Ctrl + V` a přetáhněte kopii do jiné oblasti.
- **Vyčistit mřížku:** použijte `Ctrl + Shift + L` pro uspořádání celého plátna jediným příkazem.
- **Rychlé připojení:** `Ctrl + Klik` na výstupní port a pak normální klik na vstupní port; FloWorks spojení vykreslí automaticky.

---

## 🎨 Přizpůsobení prostředí

FloWorks se přizpůsobí vám, ne naopak.

### Změna motivu za běhu
Z horního panelu, menu **Zobrazit → Motiv**, vyberte mezi světlým, tmavým a dalšími. Rozhraní se změní **okamžitě**, bez restartu a bez ztráty toku.

### Velikost písma
V **Zobrazit → Velikost písma** vyberte předdefinovanou nebo vlastní hodnotu. Celé rozhraní se okamžitě přizpůsobí.

### Jazyk
V **Zobrazit → Jazyk** vyberte požadovaný jazyk. FloWorks podporuje **změnu za běhu**: menu, tlačítka a zprávy se přeloží bez restartu aplikace.

---

## ❗ Řešení běžných problémů

| Problém | Možná příčina | Řešení |
|----------|---------------|----------|
| Tok se nespustí | Některé uzly nejsou nakonfigurovány nebo jsou přerušená připojení | Zkontrolujte, že všechny uzly mají platné parametry a že připojení jsou mezi kompatibilními porty |
| Graf se neaktualizuje | Tok je pozastaven nebo nejsou žádná data | Ujistěte se, že jste stiskli `F5` nebo ▶, a že zdrojové uzly generují data |
| Nelze propojit dva uzly | Porty jsou různých typů | Ověřte, že oba porty jsou **datové** nebo oba **řídicí** |
| Program je pomalý s velkými toky | Příliš mnoho uzlů nebo grafů v reálném čase | Zavřete nepoužívané analytické panely nebo snižte vzorkovací frekvenci zdrojových uzlů |
| Motiv se nezmění | Některé widgety nemusí být zaregistrovány | Restartujte aplikaci a zkuste to znovu (v budoucích verzích bude vyřešeno) |

---

## 🧪 Rychlé praktické příklady

Kromě úvodního sinusového toku vyzkoušejte tyto mini-projekty pro zvládnutí FloWorks:

| Příklad | Zapojené uzly | Očekávaný výsledek |
|---------|-------------------|--------------------|
| **Dolní propust** | Generátor → Filtr → Vizualizér grafů | Uvidíte filtrovaný signál |
| **Simulovaná akvizice** | Generátor → Analyzátor THD | Hodnota harmonického zkreslení signálu |
| **Manuální kontrola** | Generátor → Inspektor dat | Tabulka s hodnotami signálu vyslaného generátorem |
| **Porovnání signálů** | Dva generátory → Sčítač → Vizualizér grafů | Výsledek operace (součet, rozdíl, násobení nebo dělení) dvou vln v jednom grafu |

Každý z těchto toků lze sestavit za méně než minutu, což ukazuje obratnost FloWorks oproti tradičnímu psaní kódu.

---

## 📚 Co dál?

| Zdroj | Popis |
|---------|-------------|
| [🗺️ Průvodce anatomií rozhraní](interface-anatomy.md) | Pochopení architektury a filozofie grafického rozhraní |
| [🗺️ Mapa kódu a architektury](philosophy.md) | Úplná struktura, manažeři, kontrakty a DPI-Awareness. |
| [🧩 Technická reference uzlů](node-reference.md) | Katalog, `ScriptNode`, vícekanálové a jak rozšířit systém. |
| [🌐 Průvodce internacionalizací](translation-guide.md) | Přidání jazyků, validace JSON a správa klíčů `tr()`. |
| [📦 Průvodce přenosným buildem](guia-ejecutable-portable.md) | PyInstaller, hooks, `--onefile`, řešení chyb a digitální podpis. |

---

!!! warning "Poznámky k kompatibilitě a použití"
    1. **Verze Pythonu:** Můžete použít 3.9+, a 64bitové systémy.
    2. **Firewall Windows:** Pokud používáte reálný hardware (osciloskop VISA/SCPI), povolte `FloWorks.exe` ve firewallu. Aplikace zobrazí vlastní dialog, pokud je připojení blokováno (systémový dialog se v režimu `--windowed` nezobrazí).
    3. **Klíčové zkratky:** `F5` (spustit), `Ctrl+S` (uložit `.sflow`), `Ctrl+Klik` (připojit), `Mezerník+klik` (volný posun), `Ctrl+Shift+L` (automatické rozvržení).
    4. **Zachování dat:** Jádro **nikdy** nepoužije `flatten()` na pole. Pracujte s lokálními kopiemi, pokud potřebujete vektorizovat.
