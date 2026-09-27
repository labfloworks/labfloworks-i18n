---
title: Průvodce internacionalizací (i18n)
description: Pokyny krok za krokem pro přidání a správu překladů ve FloWorks
---

# 🌐 Průvodce internacionalizací (i18n)

Tento dokument vysvětluje, jak přidat do FloWorks nový jazyk a efektivně spravovat soubory překladů.

---

## ➕ Jak přidat nový jazyk

### Krok 1: Vytvoření souboru JSON
Přejděte do složky `locales/`. Zkopírujte `en.json` a přejmenujte jej pomocí příslušného dvoupísmenného kódu [ISO 639-1](https://cs.wikipedia.org/wiki/ISO_639-1) (např. `fr.json` pro francouzštinu, `de.json` pro němčinu).

### Krok 2: Přeložení řetězců
Otevřete nový soubor JSON v textovém editoru.

!!! warning "Neměňte klíče"
    **Nikdy neměňte klíče** (levou stranu každého páru). Překládejte pouze hodnoty (pravou stranu).

**Původní (`en.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "File",
    "edit": "Edit"
  }
}
```

**Přeložený příklad (`es.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "Archivo",
    "edit": "Editar"
  }
}
```
Ujistěte se, že kořenový klíč `"language_name"` obsahuje rodové jméno jazyka (např. `"Français"`, `"Deutsch"`, `"Español"`).

### Krok 3: Ověření JSON
Ověřte, že je soubor platný JSON (bez koncových čárek, správných uvozovek, vhodných escape sekvencí). Můžete použít online validátory jako [JSONLint](https://jsonlint.com) nebo spustit:
```bash
python -m json.tool locales/es.json
```

### Krok 4: Testování nového jazyka
1. Spusťte FloWorks.
2. Přejděte na **INFO → Jazyk** a vyberte nový jazyk.
3. Ověřte, že se všechny prvky rozhraní okamžitě aktualizují (nabídky, panely, dialogy, štítky uzlů atd.).

### Krok 5: Automatická detekce (volitelné)
Pokud místní nastavení systému uživatele odpovídá kódu nového jazyka, FloWorks jej automaticky použije při prvním spuštění (pokud nebylo dříve uloženo nastavení v `QSettings`).

---

## 🌍 Dostupné jazyky
- **Angličtina** (`en`) – Základní / záložní jazyk
- **Španělština** (`es`)

---

## ⚙️ Důležité poznámky a osvědčené postupy

!!! info "Záložní mechanismus"
    Základní jazyk je **angličtina**. Pokud v překladatelském souboru chybí klíč, FloWorks automaticky použije anglický řetězec jako zálohu.

!!! warning "Prevence přetečení UI"
    Udržujte překlady stručné, abyste zabránili rozpadu rozvržení. Pokud je přeložený text výrazně delší, zvažte zkrácení nebo spoléhněte se na systém motivů pro dynamické škálování.

!!! tip "Zachování HTML a placeholderů"
    - **HTML značky:** Zachovejte vechny HTML značky přesně tak, jak jsou (např. `<h3>`, `<b>`, `<pre>`, `<br>`).
    - **Placeholdery:** Udržujte syntaxi `{variable}` tam, kde se používá (např. `"Idioma cambiado a: {name} ({code})"`). Nepřeskupujte je ani neodstraňujte.

---

## 🔗 Související dokumentace
- [📖 Mapa kódu a architektura](architecture-ii.md)
- [📦 Průvodce sestavením a distribucí](build.md)
- [🧩 Reference uzlů](node-reference.md)
