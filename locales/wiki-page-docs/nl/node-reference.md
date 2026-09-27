---
title: Technische knooppuntreferentie
description: Bijgewerkt catalogus, extensiecontracten en geavanceerde mogelijkheden van het FloWorks-knooppuntsysteem
---

# 🧩 Technische knooppuntreferentie

FloWorks is niet afhankelijk van een statische catalogus. Het gebruikt een **dynamisch registratiesysteem** op basis van duidelijke contracten. Dit maakt het mogelijk om het platform uit te breiden zonder de topologische engine aan te passen. Hieronder staat de geïmplementeerde catalogus, de werkelijke technische mogelijkheden en het protocol voor veilige uitbreiding.

---

## 📂 Kern categorieën

=== "📦 Weergave per laag"
    <div class="grid cards" markdown>

    - **📥 Bronnen/Invoer**
      Genereren of vastleggen van initiële signalen. Ondersteunen geïntegreerde simulatie, echte hardware (VISA/SCPI) en multikanaalmodus.
    - **⚙️ Verwerking**
      Transformeren, combineren of analyseren van gegevens. Behouden dimensionaliteit en interpoleren automatisch indien nodig.
    - **🔀 Controle/Stroom**
      Vertakken, itereren of voorwaardelijk Uitvoeren. Bevatten native ondersteuning voor activeringssignalen.
    - **🐍 Scripting/Geavanceerd**
      Voeren dynamische Python-code uit met parametrische poorten (`# @param`), dynamische poorten (`# @input`/`# @output`) en statuspersistentie (`persist`).
    - **🔌 Hardware/Instrumentatie**
      Interfaces voor oscilloscopen, LCR-meters en generatoren.
    - **📤 Uitvoer/Export**
      Visualiseren, exporteren of archiveren van resultaten. Ondersteunen visuele thema's, gebruikersprofielen en professionele formaten (PNG/PDF/SVG).

    </div>

---

## 📋 Geïmplementeerde technische catalogus

| Knoop | Type | Hoofdverantwoordelijkheid | Belangrijkste kenmerken |
|-------|------|---------------------------|------------------------|
| `SumNode` | Verwerking | Rekenkundige operator (+, -, *, /) voor twee invoeren. | Interpoleert automatisch signalen van verschillende resolutie (FFTs). Behoudt dimensionaliteit. |
| `RhombusNode` | Controle | Conditioneel (vertakking Ja/Nee). | Twee uitvoerpoorten. Evalueert voorwaarde op drempel of booleaanse logica. |
| `TriggerNode` | Controle | Iterator/accumulator met externe activering. | Ontvangt `(x, y, "trigger")`. Accumuleert tot N iteraties en emitteert gestapeld/gemiddeld resultaat. |
| `ScriptNode` | Geavanceerd | Geïntegreerde Python-scriptomgeving. | QScintilla, autocompletie, `# @param`, dynamische poorten, `persist`, sjablonen, foutconsole, externe interpreter met timeout. |
| `OscilloscopeNode` | Hardware | Opname van oscilloscopen (SDS) of LCR-meters. | Simulatiemodus, geïntegreerde firewall-dialoog, **multikanaalondersteuning** (`out_primary`, `out_secondary`), menu "Toon kanaal". |
| `GeneratorNode` | Bron | Verzendt signalen naar generatoren (SDG) of simuleert uitgangen. | Modulatie/sweep-configuratie, geïntegreerde simulatiedialoog. |
| `GraphExporterNode` | Uitvoer | Professionele grafiekexporteur. | Configuratie via dubbelklik, aangepaste assen, thema's, opgeslagen profielen, etc. |

---

## 🔍 ScriptNode: Essentiële mogelijkheden

> **🐍 Geïntegreerde scriptomgeving**
>
> - **Geïntegreerde code-editor:** Basis syntax highlighting, regelnummers en code-vouwing.
> - **Dynamisch parameterpaneel:** Richtlijnen `# @param NAAM : type = waarde` injecteren bewerkbare bedieningselementen (spinbox, tekstveld etc.) in het zijpaneel.
> - **Dynamische poorten:** `# @input naam` en `# @output naam` maken poorten in realtime. Het script ontvangt een `inputs`-woordenboek en retourneert `outputs`.
> - **Statuspersistentie:** Globaal `persist`-woordenboek dat waarden tussen Uitvoeringen behoudt.
> - **Sjablonen en Import/Export:** Dropdown-menu met basisscripts. De gebruiker kan scripts opslaan in `nodes/script_node/templates/` of externe `.py`-bestanden importeren/exporteren.
> - **Geïntegreerde foutconsole:** Toont syntax-/runtimefouten met exacte regelmarkering in de editor.
> - **Hulp en i18n:** Contextuele tooltips, `?`-knop met snelle gids, en alle teksten gebruiken `tr()` voor vertaling.
> - **Externe interpreter met timeout:** Configureerbaar pad (`# @python_path` of knop "Bladeren…"). Geïsoleerde Uitvoering met tijdslimiet en fallback naar interne interpreter.
> - **Volledige serialisatie:** Slaat script, parameters, dynamische poorten en `persist`-status op. Bij het laden van een `.sflow` worden poorten en parameters automatisch gereconstrueerd.

---

## 📚 Gerelateerde bronnen

- [📖 Codekaart en architectuur](architecture-ii.md) → Verantwoordelijkheden per module en werkstromen.
- [🌐 Internationalisatie-gids (i18n)](i18n.md) → Hoe talen toe te voegen en `tr()`-sleutels te beheren.
- [🛠️ Een nieuwe knoop toevoegen (handleiding)](adding-a-new-node.md) → Stapsgewijs met praktische voorbeelden.
- [📦 Build- en distributiegids](build.md) → PyInstaller-verpakking, hooks en digitale handtekeningen.

---

💡 **Ontbreekt er een knoop in deze catalogus?**
FloWorks is ontworpen voor uitbreidbaarheid. Als je een knoop nodig hebt die niet bestaat, maak deze dan volgens het `BaseNode`-contract en registreer deze. De community en de toekomstige marketplace zullen het ecosysteem continu uitbreiden zonder compatibiliteit te verbreken.
