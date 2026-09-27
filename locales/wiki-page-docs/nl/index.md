---
title: FloWorks
description: Universeel visueel laboratorium voor signaalverwerking, wetenschappelijke instrumentatie en automatisering.
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## Universeel visueel laboratorium voor signalen, instrumentatie en AI

Wetenschappelijke verwerking • DSP • VISA/SCPI • Automatisering • Machine Learning

![FloWorks Schermafbeelding](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[Eerste stappen met FloWorks](getting-started.md){ .md-button }
[Anatomie van de interface](interface-anatomy.md){ .md-button .md-button--primary }
[Filosofie](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## Wat is FloWorks?
FloWorks is een **visueel open-source laboratorium** (Python + PySide6) waarin je systemen bouwt door blokken (knopen) te verbinden in plaats van regels code te schrijven.

Stel je een digitaal Canvas voor waar je signaalgeneratoren, wiskundige filters, hardwarecontrollers (VISA/SCPI) en modellen voor Kunstmatige Intelligentie met virtuele kabels verbindt. Alles is gebaseerd op de **datastroom**: je verbindt de uitgang van het ene blok met de ingang van het andere om informatie te verwerken, apparaten te automatiseren of resultaten in realtime te analyseren.

Het is gericht op studenten, onderzoekers, ingenieurs en iedereen die op een intuïtieve manier wil experimenteren, leren of prototypen van complexe systemen wil maken, zonder de drempel van traditioneel programmeren.

### Missie
Het experimental workflow centraliseren in één visueel, open en toegankelijk instrument. We willen dat gebruikers zich richten op *experimenteren en ontdekken*, niet op het worstelen met softwarecomplexiteit of licentiekosten.

### Visie
Een wereld waarin de enige barrière tussen een experimenteel idee en de uitvoering ervan de nieuwsgierigheid van de experimentator is. FloWorks streeft ernaar het referentieplatform voor wetenschap en techniek te worden, gebouwd door en voor de wereldwijde gemeenschap, en de muren van gesloten tools af te breken.

### Principes
* **Totale Vrijheid (MIT License):** Kennis en tools moeten vrij en toegankelijk zijn voor iedereen.
* **Oneindige Uitbreidbaarheid:** Als een blok ontbreekt, kan iedereen het maken en integreren in het ecosysteem met Python.
* **Visuele Transparantie:** Elke stap van het proces kan grafisch worden geïnspecteerd, gedebugd en begrepen.
* **Koppeling met de Echte Wereld:** Het is niet alleen simulatie; het maakt directe besturing van echte wetenschappelijke instrumentatie vanaf het Canvas mogelijk.

In tegenstelling tot gesloten of sterk gespecialiseerde tools, is FloWorks ontworpen als een modulair, uitbreidbaar ecosysteem waarin elke component een herbruikbare en verbindbare knoop is.

---

## Belangrijkste mogelijkheden

<div class="grid cards" markdown>

-   **:material-puzzle-outline: Uitbreidbaar Knoop-ecosysteem**

    Technische catalogus georganiseerd in lagen: Bronnen, Verwerking, Besturing, Hardware en Scripting.

    Dynamische registratie, declaratieve serialisatie en duidelijke contracten voor snelle ontwikkeling.

    [:material-arrow-right: Knooppuntreferentie](node-reference.md)

-   **:material-connection: VISA/SCPI-integratie**

    Directe verbinding met oscilloscopen, LCR-meters en generatoren.

    Multikanaals ondersteuning, geïntegreerde simulatie via `PyVISA-py` en firewallbeheer in draagbare modus.

    [:material-arrow-right: Instrumentatie](instrumentation.md)

-   **:material-package-variant-closed: Draagbaar `.sflow`-formaat**

    Zelfstandig ZIP-standaard met JSON-graaf, `.npy`-arrays en metadata.

    Totale reproduceerbaarheid van experimenten en automatische DPI-normalisatie.

    [:material-arrow-right: .sflow-formaat](sflow-format.md)

-   **:material-translate: Geavanceerde internationalisering**

    Taalwisseling on-the-fly zonder de app te herstarten.

    Hiërarchische JSON-vertalingen en persistentie van voorkeuren.

    [:material-arrow-right: i18n-gids](translation-guide.md)

-   **:material-tools: SDK en snelle ontwikkeling**

    Basissjabloon (`template_node.py`), serialisatiemixin en stapsgewijze handleidingen.

    Architectuur gereed voor plug-ins en community-uitbreiding.

    [:material-arrow-right: Knopen maken](adding-a-new-node.md)

</div>

---

## Toepassingsgebieden

| Gebied | Toepassingen |
|--------|--------------|
| 🎓 **Educatie** | Natuurkunde, elektronica, wiskunde, STEM-laboratoria |
| ⚙️ **Engineering** | DSP, besturing, instrumentatie, metrologie |
| 🤖 **AI** | ML, optimalisatie, hybride pipelines |
| 🔬 **Onderzoek** | Automatisering en data-acquisitie |
| 🔌 **Hardware** | VISA/SCPI, simulatie en hybride systemen |

---

!!! tip "Nieuw bij FloWorks?"

    Begin met de sectie **Eerste stappen met FloWorks**, lees dan **Anatomie van de interface** om de architectuur van de grafische interface te begrijpen, en verken tot slot de **Algemene Architectuur** om de datastroom en de structuur van de topologische engine te doorgronden.

---

!!! info "Open Core-model"

    FloWorks gebruikt een **Free/Open Core**-model onder de **MIT License**.

    De kern blijft vrij en open, terwijl toekomstige enterprise-, curriculum- of marketplace-uitbreidingen optioneel zullen zijn.

---

<div markdown="1" style="text-align: center;">

## FloWorks

Visuele verwerking • Instrumentatie • Wetenschap • AI

<small>Documentatie gebouwd met MkDocs Material</small>

</div>
