---
title: FloWorks
description: Univerzální vizuální laboratoř pro zpracování signálů, vědeckou instrumentaci a automatizaci.
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## Univerzální vizuální laboratoř pro signály, instrumentaci a AI

Vědecké zpracování • DSP • VISA/SCPI • Automatizace • Machine Learning

![Snímek FloWorks](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[První kroky s FloWorks](getting-started.md){ .md-button }
[Anatomie rozhraní](interface-anatomy.md){ .md-button .md-button--primary }
[Filozofie](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## Co je FloWorks?
FloWorks je **vizuální open-source laboratoř** (Python + PySide6), kde stavíte systémy propojováním bloků (uzlů) namísto psaní řádků kódu.

Představte si digitální Plátno, na kterém spojujete generátory signálů, matematické filtry, řadiče hardwaru (VISA/SCPI) a modely umělé inteligence pomocí virtuálních kabelů. Vše je založeno na **toku dat**: propojíte výstup jednoho bloku se vstupem druhého pro zpracování informací, automatizaci zařízení nebo analýzu výsledků v reálném čase.

Je určen pro studenty, výzkumníky, inženýry a každého, kdo chce experimentovat, učit se nebo vytvářet prototypy složitých systémů intuitivním způsobem, bez bariéry tradičního programování.

### Mise
Centralizovat experimentální pracovní postup v jednom vizuálním, otevřeném a dostupném nástroji. Chceme, aby se uživatelé soustředili na *experimentování a objevování*, nikoli na boj s komplexností softwaru nebo náklady na licence.

### Vize
Svět, kde jedinou bariérou mezi experimentální myšlenkou a její realizací je zvědavost experimentátora. FloWorks usiluje o to stát se referenční platformou pro vědu a techniku, vybudovanou komunitou a pro komunitu, a odstranit tak zdi uzavřených nástrojů.

### Principy
* **Absolutní Svoboda:** Znalosti a nástroje musí být dostupné všem. FloWorks je zdarma k použití a zavazuje se k otevřenému a rozšiřitelnému jádru.
* **Nekonečná rozšiřitelnost:** Pokud chybí blok, může ho kdokoli vytvořit a integrovat do ekosystému pomocí Pythonu.
* **Vizuální transparentnost:** Každý krok procesu lze prohlédnout, ladit a pochopit graficky.
* **Propojení s reálným světem:** Nejde jen o simulaci; umožňuje ovládat skutečnou vědeckou instrumentaci přímo z Plátna.

Na rozdíl od uzavřených nebo vysoce specializovaných nástrojů je FloWorks navržen jako modulární a rozšiřitelný ekosystém, kde je každá komponenta znovupoužitelný a propojitelný uzel.

---

## Hlavní schopnosti

<div class="grid cards" markdown>

-   **:material-puzzle-outline: Rozšiřitelný ekosystém uzlů**

    Technický katalog uspořádaný do vrstev: Zdroje, Zpracování, Řízení, Hardware a Skriptování.

    Dynamická registrace, deklarativní serializace a jasné kontrakty pro rychlý vývoj.

    [:material-arrow-right: Reference uzlů](node-reference.md)

-   **:material-connection: Integrace VISA/SCPI**

    Přímé připojení k osciloskopům, LCR měřičům a generátorům.

    Vícekanálová podpora, integrovaná simulace přes `PyVISA-py` a správa firewallu v přenosném režimu.

    [:material-arrow-right: Instrumentace](instrumentation.md)

-   **:material-package-variant-closed: Přenosný formát `.sflow`**

    Standard ZIP se samostatným obsahem s grafem JSON, polemi `.npy` a metadaty.

    Úplná reprodukovatelnost experimentů a automatická normalizace DPI.

    [:material-arrow-right: Formát .sflow](sflow-format.md)

-   **:material-translate: Pokročilá internacionalizace**

    Změna jazyka za běhu bez restartu aplikace.

    Hierarchické JSON překlady a persistence preferencí.

    [:material-arrow-right: Průvodce i18n](translation-guide.md)

-   **:material-tools: SDK a rychlý vývoj**

    Základní šablona (`template_node.py`), mixin serializace a návody krok za krokem.

    Architektura připravená na pluginy a komunitní rozšiřování.

    [:material-arrow-right: Vytvořit uzly](adding-a-new-node.md)

</div>

---

## Oblasti využití

| Oblast | Aplikace |
|--------|----------|
| 🎓 **Vzdělávání** | Fyzika, elektronika, matematika, laboratoře STEM |
| ⚙️ **Inženýrství** | DSP, řízení, instrumentace, metrologie |
| 🤖 **AI** | ML, optimalizace, hybridní pipeline |
| 🔬 **Výzkum** | Automatizace a akvizice dat |
| 🔌 **Hardware** | VISA/SCPI, simulace a hybridní systémy |

---

!!! tip "Nový v FloWorks?"

    Začněte sekcí **První kroky s FloWorks**, pak **Anatomie rozhraní** pro pochopení architektury grafického rozhraní a nakonec prozkoumejte **Obecnou architekturu** pro pochopení toku dat a struktury topologického enginu.

---

<div markdown="1" style="text-align: center;">

## FloWorks

Vizuální zpracování • Instrumentace • Věda • AI

<small>Dokumentace vytvořena pomocí MkDocs Material</small>

</div>
