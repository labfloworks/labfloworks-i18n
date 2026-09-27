---
title: Architectuur van FloWorks
description: Overzicht van componenten en interne werking voor de eindgebruiker
---

# Architectuur van FloWorks – Visie voor de gebruiker

FloWorks is een desktoptoepassing waarmee je signaalverwerkingsketens kunt bouwen via stroomdiagrammen. Je verbindt blokken (knopen) op een interactief canvas en bekijkt de resultaten in real-time. Om dit mogelijk te maken, is de toepassing georganiseerd in verschillende modules die samenwerken. Hieronder wordt, zonder technische details, uitgelegd wat elk onderdeel doet en hoe ze met elkaar samenhangen.

---

## Algemene structuur

De toepassing bestaat uit de volgende functionele gebieden:

| Gebied | Wat doet het? |
|------|------------|
| **Opstarten en hoofdvenster** | Start het programma, toont het venster, de menu's en coördineert alle gebruikersacties. |
| **Uitvoeringsmotor** | Berekent de volgorde waarin knopen moeten worden uitgevoerd, detecteert afhankelijkheden en lussen, en verzendt gegevens van de ene knoop naar de andere. |
| **Scène en diagram** | Beheert het canvas waarop je knopen plaatst, de verbindingen ertussen, de plaknotities en de acties ongedaan maken/opnieuw uitvoeren. |
| **Knopen en verwerking** | Bevat alle typen blokken die je kunt gebruiken: signaalbronnen, wiskundige bewerkingen, aangepaste scripts, grafiekexport, enz. |
| **Visuele connectoren** | Tekent de lijnen die knopen verbinden (zachte curves of orthogonale paden), animeert ze om de gegevensstroom te tonen en voorkomt overlappingen. |
| **Gebruikersinterface** | Omvat de diagramweergave (zoom, scroll), de werkbalk, de parameterentabel, de analysepaneel (statistieken, cursor) en de configuratievensters. |
| **Ondersteuning voor echte hardware** | Maakt communicatie mogelijk met laboratoriuminstrumenten (oscilloscopen, generatoren, LCR-multimeters) voor het verkrijgen of genereren van echte signalen. |
| **Grafiekexport** | Genereert afbeeldingen van hoge kwaliteit (PNG, PDF, SVG) met volledige visuele aanpassing. |
| **Thema's en uiterlijk** | Wijzigt het uiterlijk van de hele toepassing (donker, licht, hoog contrast) en laat je de lettergrootte aanpassen. |
| **Talen** | Vertaalt de hele interface naar meerdere talen en laat je de taal direct wijzigen. |
| **Projectbeheer** | Slaat en opent `.sflow`-bestanden met het volledige diagram, inclusief configuraties, scripts en resultaten. |
| **Testen en diagnose** | Interne hulpmiddelen om de correcte werking te verifiëren (niet zichtbaar voor de eindgebruiker). |

---

## Hoe het intern werkt

### Opstarten en hoofdvenster
Bij het openen van FloWorks wordt de grafische omgeving geconfigureerd, wordt de pixeldichtheid van het scherm gedetecteerd (zodat alles scherp lijkt op 4K- of normale monitoren) en wordt het hoofdvenster weergegeven. Dit venster centraliseert alle elementen: het tekengebied, de menu's, de werkbalk en de zijpanelen.

### Stroommotor
Wanneer je op "Uitvoeren" drukt (of op F5), doorloopt een interne motor alle knopen in de juiste volgorde, rekening houdend met de verbindingen. Hij weet welke knopen van anderen afhankelijk zijn en voorkomt oneindige lussen. Hij ondersteunt knopen die meerdere benoemde ingangen ontvangen en meerdere uitgangen produceren. Gegevens reizen tussen de knopen zonder hun oorspronkelijke structuur te verliezen.

### Diagramscène
Het canvas waarop je diagrammen bouwt, is een intelligente scène:

- Hiermee kun je knopen toevoegen, verplaatsen, verbinden en selecteren.
- Ondersteunt onbeperkt ongedaan maken en opnieuw uitvoeren voor elke actie.
- Bevat verstelbare plaknotities die je vrij kunt positioneren en die met het project worden opgeslagen.
- Heeft een automatische organisator die knopen netjes opnieuw positioneert (met Ctrl+Shift+L).
- Bij het opslaan wordt het volledige diagram verpakt in een `.sflow`-bestand dat de beschrijvingen van de knopen, de verbindingen, de notities en de bijbehorende numerieke gegevens bevat.

### Connectoren
De lijnen die knopen verbinden, worden getekend als zachte curves of orthogonale paden. Een subtiele animatie van stippen of strepen geeft de richting van de stroom aan. Een baanbeheerder voorkomt dat meerdere verbindingen tussen dezelfde knopen elkaar overlappen; deze worden automatisch gescheiden zodat alles leesbaar is.

### Knooptypen
Knopen zijn de fundamentele bouwstenen. Ze zijn gegroepeerd in drie categorieën:

- **Bronnen** – Genereren signalen. Ze kunnen golven simuleren (sinus, blok, enz.) of echte gegevens lezen van een aangesloten oscilloscoop of multimeter. Ze ondersteunen meerdere kanalen tegelijk (bijvoorbeeld impedantie en fase van een LCR).
- **Verwerking** – Transformeert gegevens. Omvat rekenkundige bewerkingen (optellen, aftrekken, vermenigvuldigen, delen), voorwaardelijke beslissingen (vertakking Ja/Nee) en een krachtige scriptknoop waarmee je je eigen Python-code kunt schrijven met visuele ondersteuning.
- **Putten** – Tonen of exporteren resultaten. De meest voorkomende is de grafiekweergave (virtuele oscilloscoop), maar er is ook een professionele grafiekexporter.

Elke knoop heeft ingangspoorten (links/boven) en uitgangspoorten (rechts/onder). Door een uitgangspoort te verbinden met een ingangspoort, stroomt het signaal ertussen.

#### Geavanceerde scriptknoop
De scriptknoop verdient speciale vermelding. Het is bedoeld voor gevorderde gebruikers die hun eigen verwerking willen toevoegen zonder FloWorks te verlaten. Het biedt:

- Een editor met syntax highlighting, autocompletion en regelnummers.
- De mogelijkheid om bewerkbare parameters te definiëren vanuit het knooppaneel zonder de code aan te raken (bijvoorbeeld een numerieke waarde die vervolgens in het script wordt gebruikt).
- Dynamische ingangs- en uitgangspoorten: door speciale opmerkingen in het script toe te voegen, kun je nieuwe connectoren maken.
- Permanente opslag: een speciale variabele (`persist`) die zijn waarde behoudt tussen uitvoeringen, handig voor accumulatoren of toestandsmachines.
- Kant-en-klare scriptsjablonen en de mogelijkheid om je eigen scripts op te slaan.
- Een geïntegreerd helpsysteem en een console die uitvoeringsfouten weergeeft.

### Gebruikersinterface
Naast het canvas omvat de interface:

- Een **werkbalk** met alle knopen georganiseerd per categorie, taalmenu, thema- en lettergroottekeuze, en toegang tot de logweergave.
- Een **parameterentabel** die informatie over de geselecteerde knopen weergeeft en mogelijke incompatibiliteiten markeert (zoals het proberen te werken met signalen van verschillende lengtes).
- **Dockbare analysepaneel**: statistieken (maximum, minimum, RMS), cursor A/B voor het meten van verschillen, en een vizier met piekmarkering.
- Een **welkomstvenster** dat zich aanpast aan de schermresolutie en startopties biedt.

### Verbinding met echte instrumenten
Als je compatibele hardware hebt (Siglent SDS-oscilloscopen, LCR-multimeters, SDG-generatoren), kan FloWorks ermee communiceren via het standaard VISA/SCPI-protocol. De configuratie gebeurt via specifieke panelen binnen de toepassing. Wanneer je een multikanaals signaal verkrijgt (bijvoorbeeld modulus en fase van een LCR), verpakt de bronknoop alle kanalen en kun je eenvoudig kiezen welke je wilt weergeven via een contextmenu.

### Professionele grafiekexport
De grafiekexporterknoop genereert afbeeldingen die klaar zijn voor rapporten of publicaties. Door erop te dubbelklikken, opent zich een venster met veel opties: je kunt kleuren, lijntypen, labels, schalen aanpassen, kiezen tussen PNG-, PDF- of SVG-formaten, en je voorkeuren opslaan als herbruikbare profielen.

### Visuele aanpassing
FloWorks bevat verschillende thema's (donker, licht, hoog contrast) die het uiterlijk van de hele toepassing direct wijzigen, zonder herstart. Bovendien kun je de globale lettergrootte aanpassen via het menu (Info → Lettergrootte), waarna alle elementen dienovereenkomstig worden herschaald, inclusief de tekst in knopen, plaknotities en grafieken.

### Taalsysteem
De toepassing detecteert automatisch de systeemtaal bij de eerste start en slaat de voorkeur op. Je kunt de taal op elk moment wijzigen via het menu; alle teksten, menu's en hulpfuncties worden direct bijgewerkt.

### Projecten en `.sflow`-bestanden
Al je werk wordt opgeslagen in één bestand met de extensie `.sflow`. Dit bestand bevat het volledige diagram: knopen, verbindingen, notities, configuraties, scripts en de gegenereerde numerieke gegevens. Je kunt het delen met andere gebruikers; bij het openen op een andere computer worden de notities en knopen automatisch aangepast aan de pixeldichtheid van dat scherm.

---

## Typische werkstromen

1. **Een eenvoudig diagram maken**
   Selecteer een bronknoop (bijv. Generator) en een Vizualizatorknoop van de werkbalk.
   Verbind de uitgang van de generator met de ingang van de vizualizer (Ctrl+klik op de uitgangspoort, dan klik op de ingangspoort).
   Druk op F5 om uit te voeren. Het signaal verschijnt in de grafiek.

2. **Een aangepast script gebruiken**
   Voeg een Scriptknoop toe.
   Schrijf je eigen Python-code in de editor; je kunt bewerkbare parameters en extra poorten definiëren.
   Verbind de ingangen en uitgangen zoals bij elke andere knoop.
   Voer de stroom uit; het script wordt verwerkt met je eigen gegevens.

3. **Gegevens van een echte oscilloscoop verkrijgen**
   Sluit het instrument aan en configureer de communicatie vanuit het paneel van de Oscilloscoopknoop.
   De knoop verkrijgt het signaal en levert het via zijn uitgangspoorten (één per kanaal).
   Verbind deze poorten met andere verwerkingsknopen of met de vizualizer.

4. **Een grafiek voor een rapport exporteren**
   Verbind het gewenste signaal met een Grafiekexporterknoop.
   Selecteer in de knoop (rechtermuisklik) om het visuele uiterlijk van de grafiek te configureren.
   Je kunt ook profielen laden/opslaan om het verkrijgen van rapportklare grafieken te versnellen, en het afbeeldingsbestand in de gewenste extensie verkrijgen.

---

## Waar dit allemaal voor dient

Deze architectuur is ontworpen om je te laten focussen op signaalanalyse zonder je zorgen te maken over de interne organisatie van het programma. Elk onderdeel heeft een duidelijke functie en werkt samen met de anderen om een vloeiende ervaring te bieden, van simulatie tot echte instrumentatie, via visuele aanpassing en export van resultaten.

Als je ooit de mogelijkheden van FloWorks wilt uitbreiden (bijvoorbeeld door nieuwe knooptypen toe te voegen of een ander instrument aan te sluiten), weet dan dat er een modulaire structuur bestaat die dit toelaat, hoewel dat het domein van ontwikkelaars is. Als eindgebruiker kun je genieten van de flexibiliteit die dit ontwerp biedt.
