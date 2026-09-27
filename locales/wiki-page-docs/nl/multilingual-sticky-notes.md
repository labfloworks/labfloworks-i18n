# Plaknotities (Sticky Notes) – Gebruikershandleiding

## Wat zijn plaknotities?

Plaknotities (of *sticky notes*) zijn kleine tekstblokken die u vrij op het diagram kunt plaatsen. Ze dienen om:

- Herinneringen, titels of uitleg direct op het Canvas toe te voegen.
- Stapsgewijze tutorials te maken die de gebruiker door uw project leiden.
- Delen van de workflow te documenteren zonder FloWorks te verlaten.
- Opmerkingen achter te laten voor uzelf of voor andere medewerkers.

Notities kunnen van grootte worden veranderd (door de hoeken te slepen), overal op het diagram worden verplaatst en worden opgeslagen met het project. Bij het openen van een `.sflow`-bestand verschijnen alle notities precies waar u ze heeft achtergelaten.

---

## Het nieuwe: meertalige notities

Plaknotities kunnen de tekst automatisch weergeven in de taal die u voor de applicatie kiest.  
In plaats van het uiteindelijke bericht in één taal te schrijven, kunt u **speciale markeringen** invoegen die vanzelf worden vertaald wanneer u de taal van FloWorks wijzigt.

Op deze manier kan dezelfde notitie worden gelezen in het Spaans, Engels of een andere beschikbare taal zonder dat u de tekst elke keer hoeft te bewerken.

---

## Hoe schrijf je een meertalige notitie

Binnenin een notitie (maak er een aan met een dubbelklik of met de knop 📝 op de Werkbalk) kunt u twee soorten markeringen gebruiken:

### 1. Met het woord `tr(…)`
Schrijf `tr("sleutel")` en vervang `sleutel` door een beschrijvende naam van de zin.

Voorbeeld:

```
tr("tutorial.paso1.titulo")
tr("tutorial.paso1.mensaje")
```

### 2. Met dubbele accolades `{{…}}`
Schrijf `{{sleutel}}` op dezelfde manier.

Voorbeeld:

```
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Beide formaten werken identiek; kies wat u het meest comfortabel vindt (u kunt ze zelfs in dezelfde notitie combineren).

> **Belangrijk**: De tekst die u ziet wanneer u de notitie bewerkt, bevat de oorspronkelijke markeringen (bijv. `{{tutorial.paso1.titulo}}`).  
> Wanneer u klaar bent met bewerken en terugkeert naar de normale weergave van het diagram, worden de markeringen vervangen door de zin vertaald naar de huidige taal van de applicatie.

---

## Gedrag bij taalwisseling

- Als u de taal wijzigt vanuit het FloWorks-menu (bijv. van Spaans naar Engels), **worden alle plaknotities die markeringen bevatten automatisch bijgewerkt**.
- Het is niet nodig om het project te sluiten en opnieuw te openen, of elke notitie handmatig te bewerken.
- Notities die alleen normale tekst bevatten (zonder markeringen) worden niet beïnvloed; deze tonen hetzelfde in elke taal.

---

## Voordelen van het gebruik van markeringen

- **Directe meertalige tutorials** – Eén notitie kan gebruikers van verschillende talen begeleiden.
- **Consistentie** – Als u de vertaling op één plaats wijzigt (het taalbestand dat uw ontwikkelteam beheert), worden alle notities die die sleutel gebruiken bijgewerkt.
- **Eenvoudig onderhoud** – U kunt de inhoud eenmaal schrijven en hergebruiken in meerdere notities.
- **Flexibiliteit** – Combineer vaste tekst met markeringen. Bijvoorbeeld:

```
🎯 STAP 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

---

## Praktisch voorbeeld: een stapsgewijze tutorial

Stel dat u een notitie wilt toevoegen die de eerste stap van een tutorial uitlegt.  
In de bewerkingsmodus schrijft u:

```
🎯 STAP 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Na het bewerken en bij gebruik van de applicatie in het Spaans ziet u:

```
🎯 PASO 1
¡Bienvenido a FloWorks!
Arrastre un nodo fuente de señal para comenzar.
```

Als u de taal naar Engels wijzigt, toont dezelfde notitie:

```
🎯 STEP 1
Welcome to FloWorks!
Drag a signal source node to begin.
```

Enzovoort voor elke andere geconfigureerde taal.

---

## Samenvatting

- Plaknotities verrijken uw diagrammen met tekstuele informatie.
- Ze kunnen nu **meertalig** zijn door gebruik te maken van de markeringen `tr("sleutel")` of `{{sleutel}}`.
- Bij het bewerken ziet u de sleutels; bij het bekijken de vertaalde tekst.
- Wijzig de applicatietaal en alle notities passen zich onmiddellijk aan.
- Perfect voor het maken van visuele documentatie, tutorials of meldingen die in meerdere talen moeten werken.

Maak gebruik van deze functionaliteit om uw projecten toegankelijker en gemakkelijker te delen!
