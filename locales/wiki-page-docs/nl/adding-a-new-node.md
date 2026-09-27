---
title: Handleiding voor het Toevoegen van een Nieuwe Knoop aan FloWorks
description: Stap-voor-stap handleiding voor het maken, registreren en integreren van aangepaste knopen in de flow-engine van FloWorks.
---

# 📘 Handleiding voor Ontwikkelaars: Hoe een Nieuwe Knoop aan FloWorks Toevoegen

Deze handleiding beschrijft het volledige proces voor het maken van een nieuw type knoop in FloWorks, waarbij wordt gezorgd voor correcte integratie met de flow-engine, de gebruikersinterface, de visuele thema's en het internationaliseringssysteem.

---

## 📋 Inhoudsopgave
- [📘 Handleiding voor Ontwikkelaars: Hoe een Nieuwe Knoop aan FloWorks Toevoegen](#-handleiding-voor-ontwikkelaars-hoe-een-nieuwe-knoop-aan-floworks-toevoegen)
  - [📋 Inhoudsopgave](#-inhoudsopgave)
  - [1. Inleiding tot de Architectuur](#1-inleiding-tot-de-architectuur)
  - [2. Het Sjabloon `template_node.py` Gebruiken](#2-het-sjabloon-template_nodepy-gebruiken)
  - [3. Stap voor Stap: Een Aangepaste Knoop Maken](#3-stap-voor-stap-een-aangepaste-knoop-maken)
    - [3.1. Het Sjabloon Kopiëren en Hernoemen](#31-het-sjabloon-kopiëren-en-hernoemen)
    - [3.2. Poorten en Labels Definiëren](#32-poorten-en-labels-definiëren)
    - [3.3. De Verwerkingslogica Implementeren](#33-de-verwerkingslogica-implementeren)
    - [3.4. Het Uiterlijk Aanpassen (Optioneel)](#34-het-uiterlijk-aanpassen-optioneel)
    - [3.5. Configureerbare Parameters Toevoegen (Optioneel)](#35-configureerbare-parameters-toevoegen-optioneel)
    - [3.6. De Knoop Serialiseerbaar Maken (configuraties opslaan / laden)](#36-de-knoop-serialiseerbaar-maken-configuraties-opslaan--laden)
  - [4. Integratie in het Systeem](#4-integratie-in-het-systeem)
  - [5. Internationalisering (i18n)](#5-internationalisering-i18n)
  - [6. Visuele Thema's](#6-visuele-themas)
  - [7. Controlelijst en Probleemoplossing](#7-controlelijst-en-probleemoplossing)
    - [✅ Controlelijst](#-controlelijst)
    - [🐛 Veelvoorkomende Problemen](#-veelvoorkomende-problemen)
  - [8. Conclusie](#8-conclusie)

---

## 1. Inleiding tot de Architectuur

FloWorks is gebouwd op PySide6 en gebruikt een model van verbindbare knopen die een signaalverwerkingsflow vertegenwoordigen.

---

## 2. Het Sjabloon `template_node.py` Gebruiken

Om het maken van nieuwe knopen te vereenvoudigen, wordt het bestand `nodes/template_node.py` aangeboden. Dit sjabloon bevat:
- Volledige ondersteuning voor internationalisering (koppeling aan `languageChanged`, methode `update_language`).
- Volledige ondersteuning voor thema's (methode `update_theme`).
- Geïntegreerde hulp in HTML-formaat met drie secties.
- Beheer van meerdere configureerbare invoer-/uitvoerpoorten.
- Meerdere uitvoeren met `get_output_for_port`.
- Visualisatie in de plot via `get_display_signal`.
- Vertaalbaar contextmenu.

Het wordt altijd aanbevolen om van dit sjabloon uit te gaan bij de ontwikkeling van een nieuwe knoop.

---

## 3. Stap voor Stap: Een Aangepaste Knoop Maken

### 3.1. Het Sjabloon Kopiëren en Hernoemen
1. Kopieer `nodes/template_node.py` onder de naam van uw nieuwe knoop, bijvoorbeeld `nodes/mi_nodo.py`.
2. Hernoem de klasse van `TemplateNode` naar iets beschrijvends, bijv. `MiNodoNode`.
3. Pas de imports aan indien nodig.

### 3.2. Poorten en Labels Definiëren
!!! warning "Belangrijk: Naamovereenkomst"
    De poortnamen in `PORTS`, `PORT_LABELS` en de sleutels van het woordenboek dat door `execute_program` wordt teruggegeven, moeten **exact gelijk** zijn (inclusief hoofd-/kleine letters). Het sjabloon bevat nu een aliassenmapping (`'data_in'` → eerste linkerpoort) voor meer robuustheid.

Bewerk het woordenboek `PORTS` aan de bovenkant van het bestand. Elke invoer heeft het formaat:
```python
"poortnaam": ("zijde", fractie)
```
- **Mogelijke zijdes:** `"left"`, `"right"`, `"top"`, `"bottom"`.
- **Fractie:** waarde tussen `0.0` en `1.0` die de positie langs de zijde aangeeft.

**Voorbeeld voor een knoop met één invoer en twee uitvoeren:**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
Het woordenboek `PORT_LABELS` bevat de tekst die naast elke poort verschijnt. Het wordt aanbevolen om vertaalsleutels te gebruiken in plaats van vaste tekst (zie sectie Internationalisering).

### 3.3. De Verwerkingslogica Implementeren
De belangrijkste methode is `execute_program(self, input_data)`. Deze methode wordt door de flow-engine aangeroepen wanneer de knoop gegevens ontvangt.

**`input_data` kan zijn:**
- `None` als er geen invoer is.
- Een tuple `(x, y)` voor tijdsignalen.
- Een 1D-array.
- Een woordenboek `{poortnaam: gegevens}` bij knopen met meerdere invoeren.

**Retourwaarde:**
- Voor knopen met een enkele uitvoer, geef de gegevens direct terug (bijv. een tuple `(x, y)`).
- Voor knopen met meerdere uitvoeren, geef een woordenboek terug waarbij de sleutels overeenkomen met de uitvoerpoortnamen die in `PORTS` zijn gedefinieerd.

```python
def execute_program(self, input_data):
    # Verwerk input_data en genereer resultaten
    resultaat_magnitude = (freq, mag)
    resultaat_phase = (freq, phase)
    return {
        "magnitude": resultaat_magnitude,
        "phase": resultaat_phase
    }
```

!!! tip "Opmerking over generieke poortnamen"
    De flow-engine kan af en toe een woordenboek doorgeven met sleutels zoals `'data_in'` in plaats van de werkelijke poortnaam (vooral als de gebruiker niet precies op de cirkel heeft geklikt). Het sjabloon bevat al code om dit geval af te handelen:
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    Dit voorkomt dat de knoop faalt door een onnauwkeurige verbinding.

Het sjabloon bevat al een voorbeeld in commentaar. Bovendien implementeert het `get_output_for_port(self, port_name)` zodat de engine elke uitvoer kan routeren:
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. Het Uiterlijk Aanpassen (Optioneel)
De methode `paint()` tekent de achtergrond, titel, status en eventuele aanvullende tekst. U kunt wijzigen:
- De kleuren (automatisch bijgewerkt via `update_theme`).
- De statustekst (met het attribuut `self._status`).
- Samenvattingsinformatie (bijv. piekmagnitude).

Het sjabloon toont een basisvoorbeeld.

### 3.5. Configureerbare Parameters Toevoegen (Optioneel)
Als uw knoop parameters vereist die door de gebruiker kunnen worden aangepast (bijv. venstergrootte, afsnijfrequentie), kunt u:
1. Attributen toevoegen in `__init__` (bijv. `self.window_size = 512`).
2. Een configuratiedialoog maken (erft van `QDialog`).
3. Het dialoog koppelen in `open_config_dialog()` (methode al aanwezig in het sjabloon).
4. De parameters bijwerken vanuit het dialoog en `self.update()` aanroepen.

### 3.6. De Knoop Serialiseerbaar Maken (configuraties opslaan / laden)
Opdat de knoop zijn parameters kan opslaan en herstellen bij kopiëren/plakken, ongedaan maken/opnieuw uitvoeren, of bij gebruik van de commando's Opslaan/Openen in het menu Bestand, moet deze erven van de serialisatiemixin en zijn attributen declareren.

1. Importeer de mixin in uw bestand:
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. Wijzig de overerving van de klasse om de mixin vóór `QGraphicsObject` op te nemen:
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
    ```
3. Definieer de lijst `SERIALISABLE` op klasseniveau, met de namen van de attributen die u wilt behouden. Ondersteunt alleen eenvoudige typen (`int`, `float`, `str`, `bool`), lijsten, woordenboeken, of NumPy-arrays (deze laatste worden automatisch opgeslagen als `.npy`-bestanden binnen `.sflow`).
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frecuencia', 'amplitud', 'configuracion']
    ```
4. Zorg ervoor dat deze attributen in `__init__` worden geïnitialiseerd:
    ```python
    self.frecuencia = 1000.0
    self.amplitud = 1.0
    self.configuracion = {'tipo': 'seno', 'fase': 0}
    ```

Hiermee hoeft u geen `serialize`/`deserialize`-methoden te schrijven; de mixin regelt automatisch het opslaan en herstellen van waarden.

Als uw knoop aanvullende logica vereist bij het laden (bijv. het opnieuw verbinden van een hardwareapparaat), kunt u `deserialize` overschrijven door eerst de bovenliggende methode aan te roepen:
```python
def deserialize(self, data):
    super().deserialize(data)   # herstelt attributen uit SERIALISABLE
    self._iniciar_dispositivo()
```

---

## 4. Integratie in het Systeem

Nadat het knoopbestand is aangemaakt, hoeft u het alleen maar in de map `nodes` te plakken, zodat het in de interface verschijnt en met de rest van het systeem werkt.

---

## 5. Internationalisering (i18n)

Alle zichtbare teksten moeten vertaalbaar zijn via `tr("sleutel", default="...")`. Het sjabloon implementeert dit al. U moet de overeenkomstige sleutels toevoegen aan de JSON-bestanden in `locales/`.

**Aanbevolen structuur:**
```json
{
   "nodes": {
     "mi_nodo": {
       "title": "Mi Nodo",
       "tooltip": "Tooltip beschrijving",
       "ports": {
         "input": "Invoer",
         "output1": "Uitvoer 1",
         "output2": "Uitvoer 2"
      },
       "status": {
         "no_data": "Geen gegevens",
         "ready": "Gereed"
      },
       "menu": {
         "show_output": "Toon uitvoer",
         "configure": "Configureren..."
      },
       "help_title": "Hulp - Mi Nodo",
       "help_html": "<h3>🎛️ Filter Node</h3>
<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_mi_nodo": "Mi Nodo"
  }
}
```

De HTML-hulp volgt het drie-sectieformaat dat voor alle knopen gemeenschappelijk is (specifieke beschrijving + "Hoe over het systeem denken" + "Sneltoetsen en trucs"). Het sjabloon bevat de structuur al in `get_help_text()`.

---

## 6. Visuele Thema's

De methode `update_theme(self, theme)` ontvangt een woordenboek met de kleuren gedefinieerd door het huidige thema. Het sjabloon werkt automatisch bij:
- Knoopachtergrond (`node_normal_bg`)
- Rand (`node_selected_border`)
- Titel- en tekstkleur (`node_normal_text`)
- Poortkleuren (`port_circle`, `port_outline`, `port_inline`, `port_text`)

Zorg ervoor dat in `MainWindow` (of `ThemeUpdater`) `node.update_theme()` wordt aangeroepen voor elke knoop wanneer het thema wijzigt.

---

## 7. Controlelijst en Probleemoplossing

### ✅ Controlelijst
- [ ] De knoop wordt correct aangemaakt vanuit de werkbalk.
- [ ] De poorten worden op de verwachte posities weergegeven en zijn detecteerbaar voor verbindingen (`Ctrl+klik`).
- [ ] Bij ontvangst van invoergegevens wordt `execute_program` aangeroepen en wordt het signaal verwerkt.
- [ ] De uitvoeren worden correct doorgegeven aan verbonden knopen.
- [ ] Het contextmenu maakt het mogelijk om het visualisatiekanaal te wijzigen (indien er meerdere uitvoeren zijn).
- [ ] Bij klikken op de knoop wordt het geselecteerde signaal in het plotwidget getekend.
- [ ] Dubbelklikken opent de hulp in het juiste formaat.
- [ ] De taal verandert correct (teksten van titels, poorten, menu's).
- [ ] Het thema verandert correct (kleuren van knoop en poorten).
- [ ] Kopiëren/plakken werkt zonder fouten.

!!! tip "Nauwkeurige poortverbinding"
    Zorg er bij het verbinden van knopen voor dat u precies op de cirkel van de doelpoort klikt. Als u op het lichaam van de knoop klikt, gebruikt het systeem een generieke naam (`'data_in'`). Het sjabloon tolereert deze namen nu, maar het is een goede gewoonte om rechtstreeks op de cirkel te klikken om de juiste routering van meerdere uitvoeren te garanderen.

### 🐛 Veelvoorkomende Problemen

| Symptoom | Mogelijke Oorzaak | Oplossing |
|---------|---------------|----------|
| De verbindingspijl ankerd niet aan de poort. | De poortcirkel heeft geen `setData(0, port_name)` of `get_port_scene_pos` is niet geïmplementeerd. | Controleer dat in `_create_ports` `circle.setData(0, port_name)` gebeurt en dat `get_port_scene_pos` die naam gebruikt. |
| De uitvoeren bereiken de verbonden knopen niet. | `execute_program` geeft geen woordenboek terug (voor meerdere uitvoeren) of `get_output_for_port` is niet geïmplementeerd. | Zorg ervoor dat `execute_program` `{poortnaam: gegevens}` teruggeeft en dat `get_output_for_port` de overeenkomstige waarde teruggeeft. |
| Bij klikken op de knoop wordt er niets getekend. | `get_display_signal` geeft geen geldige tuple `(x, y)` terug of `display_channel` komt niet overeen met een bestaande uitvoer. | Controleer dat `get_display_signal` het geselecteerde kanaal gebruikt en dat de gegevens NumPy-arrays zijn. |
| Teksten worden niet bijgewerkt bij taalwijziging. | Het signaal `languageChanged` is niet verbonden of `update_language` werkt elementen niet bij. | Controleer de verbinding in `__init__`: `language_manager.languageChanged.connect(self.update_language)`. |
| Het thema wordt niet toegepast. | `update_theme` wordt niet aangeroepen bij het maken van de knoop of bij themawijziging. | In `MainWindow`, na het maken van de knoop, roep `node.update_theme(self.theme_manager.current_theme())` aan. |
| De pijl wijst naar het midden van de knoop. | Er is op het lichaam geklikt in plaats van op de cirkel, of de naam komt niet overeen met `PORTS`. | Klik rechtstreeks op de cirkel. Verifieer dat `get_port_scene_pos` de aliassenmapping heeft. |
| `NameError: name 'self' is not defined` bij importeren. | Instantie-attributen zijn buiten `__init__` gedeclareerd. | Alle attributen zoals `self.mijn_parameter` moeten binnen `__init__` worden gedefinieerd. |
| Parameters gaan verloren bij kopiëren/openen `.sflow`. | De knoop erft niet van `SerializableMixin` of `SERIALISABLE` is niet gedefinieerd. | Implementeer stap 3.6 van deze handleiding. |

---

## 8. Conclusie

Door deze handleiding te volgen en het sjabloon `template_node.py` te gebruiken, kunt u op een efficiënte en consistente manier met de rest van het systeem nieuwe knopen aan FloWorks toevoegen. Vergeet niet om altijd compatibiliteit met i18n en thema's te behouden voor een professionele gebruikerservaring.

Doe mee met uw eigen knopen!
