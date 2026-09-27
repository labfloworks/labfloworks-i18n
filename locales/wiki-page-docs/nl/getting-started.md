---
title: Aan de slag met FloWorks
description: Snelle handleiding voor het instellen van de omgeving, het uitvoeren van je eerste stroom en toegang tot de draagbare versie.
---

# 🚀 Aan de slag met FloWorks

Deze gids leidt je van nul tot het draaien van je eerste signaalverwerkingsstroom. FloWorks is een stroomdiagramtoepassing voor signalen, gebouwd met Python en PySide6, die echte hardware (VISA/SCPI), geïntegreerde simulatie, geavanceerde scripting en taalwisseling tijdens het uitvoeren ondersteunt.

---

## 🌊 Je eerste voorbeeldstroom

Laten we een eenvoudige stroom maken: een sinusvormig signaal genereren en dit in real-time visualiseren.

1. **Knoppen toevoegen**
   Selecteer in de bovenste werkbalk `Bron` → selecteer `Geavanceerde Signaalgenerator`. Vervolgens, vanuit `Verwerking` → selecteer bijvoorbeeld `Spectraal`.
2. **Verbinden**
   Druk op `Ctrl+Klik` op de uitgangspoort (`rechts`) van de generator. Klik vervolgens op de invoerpoort (`links`) van de oscilloscoop. Of klik eenvoudigweg op de uitgangspoort en sleep (ingedrukt houden) naar de invoerpoort van de volgende knoop.
3. **Configureren (optioneel)**
   Klik op een knoop; in het linkerzijpaneel verschijnt een editor met de parameters van de geselecteerde knoop om de bedrijfsomstandigheden aan te passen. In het onderste deel is er een grafische weergave die de gegevens die door de knopen worden gegenereerd of verkregen visueel weergeeft.
4. **Uitvoeren**
   Druk op `F5` of de knop ▶ in de werkbalk. De topologische engine berekent de uitvoervolgorde, verwerkt de gegevens en je ziet de golf in het grafische paneel. **De connector wordt geanimeerd om de actieve stroom aan te geven!**

---

## 🧠 Poorten begrijpen: categorieën per kleur

In FloWorks behoort elke poort tot een **functionele categorie** die wordt geïdentificeerd door een kleur. Geldige verbindingen worden **altijd tussen poorten van dezelfde kleur** gemaakt: een uitgang van een categorie wordt uitsluitend verbonden met een invoer van dezelfde categorie. Bovendien neemt de verbindingslijn automatisch de kleur over van de poorten die hij verbindt, waardoor visuele aflezing wordt vergemakkelijkt.

| Type | Kleur | Doel | Typisch voorbeeld |
|------|-------|-----------|----------------|
| `control` | Wit | Controlestroom / activering. | Startsignaal naar een acquisitieknoop. |
| `exec` | Grijs | Uitvoering van bewerkingen of stappen. | Triggeren van een functie of callback. |
| `data` | Groen | Generieke gegevens / numerieke signalen. | Uitgang van een generator of sensor. |
| `int` | Blauw | Gehele getallen. | Index, buffergrootte, ID. |
| `float` | Cyaan | Drijvende-kommagetallen. | Amplitude, frequentie, drempel. |
| `string` | Paars | Tekenreeksen. | Bestandsnaam, label. |
| `bool` | Roze | Booleaanse waarden (`True`/`False`). | Statusvlag, inschakeling. |
| `array` | Donkerblauw | Arrays / vectoren. | Multikanaals signaal, lijst met samples. |
| `trigger` | Oranje | Triggers / discrete gebeurtenissen. | Synchronisatiepuls, flank. |

**Gouden regel:**

- Alleen poorten met **exact dezelfde kleur** worden verbonden (uitgang ↔ invoer van dezelfde categorie).
- Het systeem voorkomt ongeldige verbindingen en benadrukt visueel compatibele poorten tijdens het slepen.
- De verbindingslijn neemt de kleur over van de verbonden poorten; zo is elk traject in één oogopslag te identificeren.

**FloWorks-filosofie:**
Gegevenspoorten **behouden de dimensionaliteit** van arrays. Er wordt nooit automatisch afvlakking toegepast: als er een matrix ingaat, komt er een matrix uit, waardoor de integriteit van je multidimensionale signalen behouden blijft.

![FloWorks](assets/tipos_de_puertos.PNG)

---

## 🖱️ Navigeren op het canvas

Beheers de werkruimte met deze gebaren:

| Actie | Hoe te doen |
|--------|--------------|
| **Zoomen** | Scrollwiel of `Ctrl + scrollwiel` |
| **Pannen (verschuiven)** | Houd `Spatie` ingedrukt en sleep, of gebruik de middelste muisknop |
| **Knoop selecteren** | Linkerklik op de knoop |
| **Meervoudige selectie** | Sleep een rechthoek met linkerklik, of `Ctrl + klik` op meerdere knopen |
| **Selectie verplaatsen** | Sleep een van de geselecteerde knopen |
| **Configuratie openen** | Dubbelklik op de knoop |

**Tip:** Het linkerzijpaneel wordt automatisch bijgewerkt met de configuratie van de geselecteerde knoop, zonder extra vensters te openen.

---

## ⚡ Sneltoetsen en geavanceerde bewegingen

Deze sneltoetsen maken van een normale gebruiker een **power user**:

| Sneltoets | Actie |
|-------|--------|
| `F5` | Stroom uitvoeren |
| `Ctrl + S` | Project opslaan (`.sflow`) |
| `Ctrl + Klik` | Knopen verbinden (klik op uitgangspoort → klik op invoerpoort) |
| `Ctrl + C` / `Ctrl + V` | Geselecteerde knopen kopiëren / plakken |
| `Ctrl + Z` / `Ctrl + Y` | Ongedaan maken / opnieuw uitvoeren |
| `Ctrl + Shift + L` | Knopen automatisch op het canvas rangschikken |
| `Del` | Geselecteerde knopen verwijderen |
| `Ctrl + A` | Alle knopen selecteren |

**Geavanceerde bewegingen:**

- **Stroom dupliceren:** selecteer een groep knopen, `Ctrl + C`, `Ctrl + V` en sleep de kopie naar een ander gebied.
- **Raster opruimen:** gebruik `Ctrl + Shift + L` om het hele canvas met één opdracht te ordenen.
- **Snel verbinden:** `Ctrl + Klik` op een uitgangspoort en dan normale klik op de invoerpoort; FloWorks tekent de verbinding automatisch.

---

## 🎨 Omgeving aanpassen

FloWorks past zich aan jou aan, niet andersom.

### Thema wijzigen tijdens uitvoeren
Vanuit de bovenste balk, menu **Weergave → Thema**, kies tussen licht, donker en andere. De interface verandert **onmiddellijk**, zonder herstart en zonder werkstroomverlies.

### Lettergrootte
In **Weergave → Lettergrootte** selecteer je een vooraf gedefinieerde of aangepaste waarde. De hele interface past zich meteen aan.

### Taal
In **Weergave → Taal** selecteer je de gewenste taal. FloWorks ondersteunt **wisseling tijdens uitvoeren**: menu's, knoppen en berichten worden vertaald zonder de toepassing te herstarten.

---

## ❗ Oplossen van veelvoorkomende problemen

| Probleem | Mogelijke oorzaak | Oplossing |
|----------|---------------|----------|
| Stroom wordt niet uitgevoerd | Er zijn niet-geconfigureerde knopen of verbroken verbindingen | Controleer dat alle knopen geldige parameters hebben en dat verbindingen tussen compatibele poorten liggen |
| Grafiek wordt niet bijgewerkt | Stroom is gepauzeerd of er vloeien geen gegevens | Zorg ervoor dat je op `F5` of ▶ hebt gedrukt, en dat bronknopen gegevens genereren |
| Kan twee knopen niet verbinden | Poorten zijn van verschillend type | Controleer dat beide poorten **gegevens** of beide **controle** zijn |
| Programma traag bij grote stromen | Te veel knopen of real-time grafieken | Sluit ongebruikte analysepanelen of verlaag de bemonsteringsfrequentie van bronknopen |
| Thema verandert niet | Sommige widgets zijn mogelijk niet geregistreerd | Herstart de toepassing en probeer opnieuw (wordt opgelost in toekomstige versies) |

---

## 🧪 Snelle praktische voorbeelden

Naast de eerste sinusvormige stroom, probeer deze mini-projecten om FloWorks onder de knie te krijgen:

| Voorbeeld | Betrokken knopen | Verwacht resultaat |
|---------|-------------------|--------------------|
| **Laagdoorlaatfilter** | Generator → Filter → Grafiekweergave | Je ziet het gefilterde signaal |
| **Gesimuleerde acquisitie** | Generator → THD-analysator | Waarde van de harmonische vervorming van het signaal |
| **Handmatige controle** | Generator → Gegevensinspecteur | Tabel met de waarden van het signaal dat door de generator wordt verzonden |
| **Signaalvergelijking** | Twee generatoren → Opteller → Grafiekweergave | Het resultaat van de bewerking (optelling, aftrekking, vermenigvuldiging of deling) van twee golven in één grafiek |

Elk van deze stromen kan in minder dan een minuut worden opgebouwd, wat de behendigheid van FloWorks ten opzichte van traditionele codering aantoont.

---

## 📚 Wat nu?

| Bron | Beschrijving |
|---------|-------------|
| [🗺️ Gids voor Anatomie van de Hoofdinterface](interface-anatomy.md) | Begrip van de architectuur en filosofie van de grafische interface |
| [🗺️ Kaart van Code en Architectuur](philosophy.md) | Volledige structuur, managers, contracten en DPI-Awareness. |
| [🧩 Technische Referentie van Knopen](node-reference.md) | Catalogus, `ScriptNode`, multikanaal en hoe het systeem uit te breiden. |
| [🌐 Handleiding voor Internationalisering](translation-guide.md) | Talen toevoegen, JSON valideren en `tr()`-sleutels beheren. |
| [📦 Handleiding voor Draagbare Build](guia-ejecutable-portable.md) | PyInstaller, hooks, `--onefile`, foutoplossing en digitale handtekening. |

---

!!! warning "Opmerkingen over compatibiliteit en gebruik"
    1. **Python-versie:** Je kunt 3.9+ en 64-bits systemen gebruiken.
    2. **Windows Firewall:** Als je echte hardware gebruikt (VISA/SCPI-oscilloscoop), sta `FloWorks.exe` toe in de firewall. De toepassing toont een aangepast dialoogvenster als de verbinding wordt geblokkeerd (het dialoogvenster van het besturingssysteem verschijnt niet in `--windowed`-modus).
    3. **Belangrijke sneltoetsen:** `F5` (uitvoeren), `Ctrl+S` (`.sflow` opslaan), `Ctrl+Klik` (verbinden), `Spatie+klik` (vrij pannen), `Ctrl+Shift+L` (automatische indeling).
    4. **Gegevensbehoud:** De engine past **nooit** `flatten()` toe op arrays. Werk met lokale kopieën als je vectorisatie nodig hebt.
