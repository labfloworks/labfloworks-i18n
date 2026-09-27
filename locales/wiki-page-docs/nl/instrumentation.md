---
title: Instrumentatie VISA/SCPI
description: Handleiding voor het aansluiten, configureren en gebruiken van echte en gesimuleerde hardware in FloWorks via de VISA/SCPI-standaard.
---

# 🔌 Instrumentatie VISA/SCPI

FloWorks integreert directe communicatie met echte laboratoriuminstrumenten via het **SCPI**-protocol (Standard Commands for Programmable Instruments) over de abstractielaag **VISA** (Virtual Instrument Software Architecture). Daarnaast biedt het simulatoren in pure Python voor het ontwikkelen, testen en delen van stromen zonder fysieke hardware.

---

## 🌐 Wat is VISA/SCPI?

| Technologie | Beschrijving |
|-------------|--------------|
| **VISA** | Standaard abstractielaag voor het fysieke interface (USB-TMC, Ethernet/LAN, GPIB, RS‑232). Maakt het mogelijk om van een echt instrument naar een gesimuleerd instrument te schakelen door alleen de resource string te wijzigen. |
| **SCPI** | Gestandaardiseerde ASCII-commandotaal voor het aansturen van generatoren, oscilloscopen, multimeters, LCR-meters, etc. Fabrikanten breiden de standaard uit, maar de basis is universeel. |
| **PyVISA** | Python-backend gebruikt door FloWorks. Ondersteunt `@py` (pure simulatie) en native backends (`@ni`, `@ivi`, `@keysight`, etc.). |

---

## ⚙️ Typische configuratie

=== "📍 Resource String"
    Standaard VISA-formaat:
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR` (USB-oscilloscoop)
    - `TCPIP0::192.168.1.100::inst0::INSTR` (LAN/Ethernet)
    - `ASRL1::INSTR` (RS-232 seriële poort)
    - `GPIB0::1::INSTR` (GPIB legacy)

=== "⏱️ Time-out en opties"
    - **Timeout**: Configureerbaar in ms. Verhoog als het instrument lange metingen of frequentiesweeps vereist.
    - **Initialisatie**: Sommige knopen staan het injecteren van aangepaste SCPI-commando's toe bij verbinding (bijv. `*CLS`, `SYST:PRES`, `:CHAN1:DISP ON`).

---

## 📡 Beschikbare hardware-knopen

<div class="grid cards" markdown>

- **🔭 SCPI-oscilloscoop**
  Vangt tijddomein-golfvormen. Ondersteunt multikanaal, automatische schaling, hardware-trigger en het menu "Toon kanaal" om signalen live te wisselen.

- **⚡ LCR-meter**
  Meet impedantie, inductie, capaciteit, weerstand en verliesfactor. Retourneert een `master_payload` met primaire en secundaire gegevens in één acquisitie.

- **🎛️ Willekeurige-functiegenerator**
  Zendt signalen naar SDG-hardware of simuleert uitgangen. Configureert modulatie (AM/FM/PM), lineaire/logaritmische sweep, burst en fase.

- **📊 Digitale multimeter (DMM)** *(In ontwikkeling)*
  SCPI-interface voor DC/AC-spannings-, stroom-, weerstands- en frequentiemetingen. Compatibel met Keithley, Agilent en Rigol.

- **🔋 Programmeerbare voeding** *(In ontwikkeling)*
  Sturing van uitgangs spanning/stroom met OVP/OCP-bescherming. Handig voor geautomatiseerde testopstellingen.

</div>

---

## 🔄 Typische werkstroom

1. **Voeg de knoop toe** aan het Canvas vanuit de Werkbalk (`Bronnen` of `Instrumenten`).
2. **Configureer de verbinding**: Selecteer backend, voer de VISA-string in en pas timeout/initialisatie aan.
3. **Koppel aan de stroom**: Verbind de instrumentuitgang met verwerkingsknopen (FFT, filters, rekenkunde) of visualisatie.
4. **Uitvoeren (`F5`)**: De topologische engine vraagt acquisitie aan, de driver parseert het SCPI-antwoord en verpakt de gegevens.
5. **Visualiseren/Exporteren**: De gegevens stromen door de graaf om verder te worden verwerkt door volgende knopen.

---

## 🛠️ Probleemoplossing

!!! warning "1. VISA vindt het instrument niet (`VI_ERROR_RSRC_NFOUND`)"
    - **Oorzaak:** Onjuiste string, losgekoppelde kabel of backend detecteert apparaat niet.
    - **Oplossing:** Voer `pyvisa-shell` of de fabrikanttool (NI MAX, Keysight Connection Expert) uit om geldige resources te tonen. Controleer gebruikersrechten.

!!! warning "2. Time-out tijdens acquisitie"
    - **Oorzaak:** Trage sweep, trigger niet voldaan of instrument bezet met andere taak.
    - **Oplossing:** Verhoog de timeout in de knoop. Controleer de triggerconfiguratie van de oscilloscoop (`AUTO` of `NORMAL`). Gebruik `*CLS` aan het begin.

!!! warning "3. Simulatie reageert niet of faalt"
    - **Oorzaak:** `PyVISA-py` is niet geïnstalleerd of er is een conflict met een andere backend.
    - **Oplossing:** `pip install pyvisa-py`. Selecteer in de knoop expliciet `@py` als backend.

!!! warning "4. SCPI-fouten (`Command Error`, `Execution Error`)"
    - **Oorzaak:** Commando niet ondersteund door firmware of onjuiste syntaxis.
    - **Oplossing:** Raadpleeg het SCPI-programmeerhandboek van je instrument. Sommige fabrikanten vereisen voorvoegsels `:` of terminators `\n`. FloWorks voegt `\n` automatisch toe, maar je kunt de terminator in de driver aanpassen.

!!! info "5. Een knoop maken voor een niet-ondersteund instrument"
    - Erf van `BaseNode` en gebruik het `DeviceBase`-patroon in `instrument/`.
    - Implementeer een `headless` driver die tuples `(x, y)` of `master_payload` retourneert.
    - Volg de [📘 Handleiding: Een nieuwe knoop toevoegen](adding-a-new-node.md) voor registratie van poorten, serialisatie en i18n.

---

## 📚 Gerelateerde bronnen

- [🧩 Technische knooppuntreferentie](node-reference.md) → Details over `oscilloscope_node`, `generator_node` en serialisatiecontracten.
- [📦 Handleiding draagbare build](guia-ejecutable-portable.md) → Firewallbeheer, `resource_path()` en PyInstaller-verpakking.
- [📘 Een nieuwe knoop toevoegen](adding-a-new-node.md) → Hoe `instrument/` uit te breiden en aangepaste drivers te registreren.
- [📄 `.sflow`-formaat](sflow-format.md) → Hoe hardwareconfiguraties en vastgelegde arrays worden bewaard.
