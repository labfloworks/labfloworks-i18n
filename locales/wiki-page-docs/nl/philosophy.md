# Filosofie

## Overzicht

FloWorks is een desktoptoepassing waarmee u ketens van signaalverwerking kunt maken via visuele stroomdiagrammen.
Sleep, verbind en configureer knopen; het resultaat wordt berekend en in real-time weergegeven.
Werk met gesimuleerde signalen of sluit echte instrumenten aan (oscilloscopen, generatoren, LCR-multimeters) zonder code te schrijven, hoewel u een krachtige scripting-omgeving tot uw beschikking heeft als u de functionaliteit wilt uitbreiden.

---

## Belangrijkste kenmerken

- **Interactieve diagrammen** – Bouw uw workflow door knopen te verbinden met lijnen die de gegevensstroom weergeven.
- **Real-time verwerking** – Elke wijziging wordt onmiddellijk weerspiegeld in de grafieken en visualisaties.
- **Simulatie en echte hardware** – Genereer testsignalen of vang gegevens rechtstreeks van laboratoriuminstrumenten.
- **Geavanceerde scriptknoop** – Voeg uw eigen Python-code in met hulp bij autocompletie, bewerkbare dynamische parameters en persistent geheugen tussen uitvoeringen.
- **Professionele visualisatie** – Signalen, spectra, spectrogrammen en grafieken van hoge kwaliteit klaar voor export.
- **Meertalig** – De interface detecteert de systeemtaal en laat u op elk moment schakelen tussen Spaans, Engels en andere talen.
- **Visuele thema's** – Donkere, lichte en hoog contrast modus om aan uw voorkeuren of toegankelijkheidsbehoeften aan te passen.
- **Volledige projectbeheer** – Sla uw werk op in `.sflow`-bestanden en herstel het precies zoals u het heeft achtergelaten, met onbeperkt ongedaan maken en opnieuw uitvoeren.

---

## Hoe te werken met FloWorks

### Knopen
Een knoop is een onderdeel van de verwerking. Ze zijn georganiseerd in drie categorieën:

- **Bronnen** – Voegen signalen in aan het begin van de stroom. Bijvoorbeeld een oscilloscoop (echt of gesimuleerd), een functiegenerator of een wiskundige bewerking.
- **Verwerking** – Transformeren de gegevens. Sommen, verschillen, voorwaarden, filters… inclusief een speciale knoop om uw eigen Python-scripts te schrijven.
- **Putten** – Tonen of exporteren de resultaten. De grafiekvisualizer en de professionele grafiekexporteur zijn de meest gebruikte.

### Verbindingen
De verbindingen tussen knopen worden getekend als gladde curves of orthogonale lijnen. Een stroomanimatie geeft u op elk moment de richting van de gegevens aan. Het systeem organiseert de kabels automatisch zodat ze elkaar niet overlappen.

### Visualisatie
Wanneer een knoop een signaal produceert, kan dit worden bekeken in het geïntegreerde grafiekpaneel. U kunt verschillende weergaven verkennen (golfvorm, spectrum, spectrogram) en de schaal aanpassen met de muis.

---

## Uitgelichte knopen
Dit zijn de minimaal onmisbare knopen die nodig zijn om de filosofie van het programma zinvol te maken.

### Signaalgeneratorknoop
Een signaalbron die door de gebruiker gedefinieerde simulaties van golfvormen kan genereren. Laat via een contextmenu toe om de gewenste golfvorm te selecteren of in te voeren.

### Scriptknoop
Een complete programmeeromgeving binnen het diagram:

- **Editor met syntax highlighting**, autocompletie en foutconsole.
- **Dynamische parameters** – Definieer bewerkbare variabelen vanuit het knooppaneel zonder de code te wijzigen.
- **Configureerbare poorten** – Voeg extra ingangen en uitgangen rechtstreeks vanuit de editor toe.
- **Persistente toestand** – Bewaar waarden tussen uitvoeringen; alles wordt samen met het project opgeslagen.

### Grafiekexporteur
Een putknoop die afbeeldingen van hoge kwaliteit genereert voor rapporten of publicaties. Laat toe om grootte, resolutie, formaat en meer te configureren.

---

## Personalisatie

- **Taal** – De toepassing detecteert automatisch de systeemtaal en slaat uw voorkeur op. U kunt deze wijzigen vanuit het menu zonder opnieuw op te starten.
- **Uiterlijk** – Kies tussen donker, licht of hoog contrast thema afhankelijk van de omgevingsverlichting of uw visuele behoeften.

---

## Projecten en bestanden

Sla uw volledige diagram op in een `.sflow`-bestand.
Bij het openen herstelt u alle knopen, verbindingen, scripts, parameters en visualisatieconfiguraties.
De acties ongedaan maken en opnieuw uitvoeren laten u toe om te experimenteren zonder angst om eerder werk te verliezen.

---

FloWorks is ontworpen zodat u zich kunt concentreren op signaalanalyse en niet op de technische details van de implementatie. Sleep, verbind en ontdek.
