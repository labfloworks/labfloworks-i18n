---
title: Správa projektů
description: Jak ukládat, otevírat, exportovat a chránit své toky ve FloWorks.
---

# 📁 Správa projektů

FloWorks ukládá vaše toky do souborů s příponou **`.sflow`**. Tyto soubory obsahují veškeré informace o projektu: uzly, připojení, konfiguraci a lepicí poznámky.

---

## Vytvoření, otevření a uložení

| Akce | Menu | Zkratka |
|--------|------|-------|
| **Nový projekt** | Soubor → Nový | `Ctrl + N` |
| **Otevřít projekt** | Soubor → Otevřít | `Ctrl + O` |
| **Uložit** | Soubor → Uložit | `Ctrl + S` |
| **Uložit jako…** | Soubor → Uložit jako… | `Ctrl + Shift + S` |

**Zlaté pravidlo:**
Toky jsou **plně kompatibilní napříč všemi verzemi** FloWorks (Core, Lite, Pro). Nepotřebujete nic konvertovat ani upravovat: stačí otevřít a spustit.

---

## Export a import

- Pro **sdílení toku** zkopírujte soubor `.sflow` na jiné zařízení.
- Pro **načtení externího toku** použijte **Soubor → Otevřít** a vyberte soubor.
- Pokud potřebujete **exportovat číselná data** (např. do CSV), použijte nástroj **Tabulka** v postranním panelu a uložte tabulku odtamtud.

---

## Obnova po neočekávaném ukončení

FloWorks **neukládá automaticky**. Proto je důležité:

- Ukládat často (`Ctrl + S`), zejména před spuštěním toků s reálným hardwarem.
- Pokud se aplikace neočekávaně ukončí, neuložené změny mohou být ztraceny.
- Pro naprostý klid si vytvořte zvyk ukládat po každé významné úpravě.

---

## Doporučená organizace

- Vytvořte složku pro každý projekt nebo klienta a ukládejte tam všechny související `.sflow`.
- Používejte **lepicí poznámky** na plátně pro dokumentaci částí toku.
- Přiřazujte **výstižné názvy uzlům** (dvojklik → název), aby bylo snazší tok najít a pochopit i týdny poté.

---

## Doporučené postupy

- Před spuštěním toku s reálnými přístroji soubor uložte.
- Pokud pracujete v týmu, používejte systém správy verzí (Git, ruční kopie), abyste nepřepsali důležité toky.
- Zálohujte toky pro kalibraci nebo diagnostiku kritických systémů.

---

> **Tip:** Dobře uspořádaný a uložený tok je základem profesionální práce ve FloWorks. Ne podceňujte sílu jasného názvu a uklizené složky.
