---
title: VISA/SCPI-Instrumentierung
description: Leitfaden zum Verbinden, Konfigurieren und Verwenden von echter und simulierter Hardware in FloWorks über den VISA/SCPI-Standard.
---

# 🔌 VISA/SCPI-Instrumentierung

FloWorks integriert die direkte Kommunikation mit echten Laborgeräten über das Protokoll **SCPI** (Standard Commands for Programmable Instruments) über die Abstraktionsschicht **VISA** (Virtual Instrument Software Architecture). Darüber hinaus bietet es rein Python-basierte Simulatoren, um Flüsse ohne physische Hardware zu entwickeln, zu testen und zu teilen.

---

## 🌐 Was ist VISA/SCPI?

| Technologie | Beschreibung |
|------------|-------------|
| **VISA** | Standardisierte Schicht, die die physische Schnittstelle (USB-TMC, Ethernet/LAN, GPIB, RS‑232) abstrahiert. Ermöglicht den Wechsel von einem echten zu einem simulierten Gerät nur durch Ändern der Verbindungszeichenkette. |
| **SCPI** | Standardisierte ASCII-Befehlssprache zur Steuerung von Generatoren, Oszilloskopen, Multimetern, LCR-Messgeräten usw. Hersteller erweitern den Standard, die Basis ist jedoch universell. |
| **PyVISA** | Von FloWorks verwendetes Python-Backend. Unterstützt `@py` (reine Simulation) und native Backends (`@ni`, `@ivi`, `@keysight` usw.). |

---

## ⚙️ Typische Konfiguration

=== "📍 Verbindungszeichenkette (Resource String)"
    Standard-VISA-Format:
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR` (USB-Oszilloskop)
    - `TCPIP0::192.168.1.100::inst0::INSTR` (LAN/Ethernet)
    - `ASRL1::INSTR` (RS-232-Seriellport)
    - `GPIB0::1::INSTR` (Legacy-GPIB)

=== "⏱️ Timeout und Optionen**"
    - **Timeout**: Konfigurierbar in ms. Erhöhen, wenn das Gerät lange Messungen oder Frequenzsweeps erfordert.
    - **Initialisierung**: Einige Knoten erlauben das Injizieren benutzerdefinierter SCPI-Befehle beim Verbinden (z. B. `*CLS`, `SYST:PRES`, `:CHAN1:DISP ON`).

---

## 📡 Verfügbare Hardware-Knoten

<div class="grid cards" markdown>

- **🔭 SCPI-Oszilloskop**  
  Erfasst Wellenformen im Zeitbereich. Unterstützt Mehrkanal, automatische Skalierung, Hardware-Trigger und das Menü „Kanal anzeigen" zum Umschalten von Signalen im laufenden Betrieb.

- **⚡ LCR-Messgerät**  
  Misst Impedanz, Induktivität, Kapazität, Widerstand und Verlustfaktor. Gibt ein `master_payload` mit primären und sekundären Daten in einem einzigen Abruf zurück.

- **🎛️ Arbiträrer Funktionsgenerator**  
  Sendet Signale an SDG-Hardware oder simuliert Ausgänge. Konfiguriert Modulation (AM/FM/PM), linearen/logarithmischen Sweep, Burst und Phase.

- **📊 Digitalmultimeter (DMM)** *(In Entwicklung)*  
  SCPI-Schnittstelle für DC/AC-Spannungs-, Strom-, Widerstands- und Frequenzmessungen. Kompatibel mit Keithley, Agilent und Rigol.

- **🔋 Programmierbares Netzgerät** *(In Entwicklung)*  
  Ausgangsspannungs-/Stromsteuerung mit OVP/OCP-Schutz. Nützlich für automatisierte Prüfstände.

</div>

---

## 🔄 Typischer Arbeitsablauf

1. **Knoten hinzufügen**: Zum Canvas aus der Toolbar (`Quellen` oder `Instrumente`) hinzufügen.
2. **Verbindung konfigurieren**: Backend auswählen, VISA-Zeichenkette eingeben und Timeout/Initialisierung anpassen.
3. **Zum Fluss verbinden**: Geräteausgang mit Verarbeitungsknoten (FFT, Filter, Arithmetik) oder Visualisierung verbinden.
4. **Ausführen (`F5`)**: Die Topologie-Engine fordert den Abruf an, der Treiber parst die SCPI-Antwort und verpackt die Daten.
5. **Visualisieren/Exportieren**: Daten fließen durch den Graphen, um von nachfolgenden Knoten verarbeitet zu werden. 

---

## 🛠️ Fehlerbehebung

!!! warning "1. VISA findet das Gerät nicht (`VI_ERROR_RSRC_NFOUND`)"
    - **Ursache:** Falsche Zeichenkette, Kabel getrennt oder Backend erkennt das Gerät nicht.
    - **Lösung:** `pyvisa-shell` oder das Hersteller-Tool (NI MAX, Keysight Connection Expert) ausführen, um gültige Ressourcen aufzulisten. Benutzerberechtigungen überprüfen.

!!! warning "2. Timeout während des Abrufs"
    - **Ursache:** Langsamer Sweep, Trigger wird nicht erfüllt oder Gerät ist mit einer anderen Aufgabe beschäftigt.
    - **Lösung:** Timeout im Knoten erhöhen. Überprüfen, ob der Oszilloskop-Trigger korrekt konfiguriert ist (`AUTO` oder `NORMAL`). `*CLS` zu Beginn verwenden.

!!! warning "3. Simulation reagiert nicht oder schlägt fehl"
    - **Ursache:** `PyVISA-py` ist nicht installiert oder es gibt einen Konflikt mit einem anderen Backend.
    - **Lösung:** `pip install pyvisa-py` ausführen. Im Knoten explizit `@py` als Backend auswählen.

!!! warning "4. SCPI-Fehler (`Command Error`, `Execution Error`)"
    - **Ursache:** Befehl wird von der Firmware nicht unterstützt oder Syntax ist falsch.
    - **Lösung:** Das SCPI-Programmierhandbuch des Geräts konsultieren. Einige Hersteller erfordern `:`-Präfixe oder `
`-Terminatoren. FloWorks fügt `
` automatisch hinzu, aber der Terminator kann im Treiber angepasst werden.

!!! info "5. Knoten für nicht unterstütztes Gerät erstellen"
    - Von `BaseNode` erben und das `DeviceBase`-Muster in `instrument/` verwenden.
    - Einen `headless`-Treiber implementieren, der Tupel `(x, y)` oder `master_payload` zurückgibt.
    - Der [📘 Leitfaden: Neuen Knoten hinzufügen](adding-a-new-node.md) folgen, um Ports, Serialisierung und i18n zu registrieren.

---

## 📚 Verwandte Ressourcen

- [🧩 Technische Knotenreferenz](node-reference.md) → Details zu `oscilloscope_node`, `generator_node` und Serialisierungsverträgen.
- [ Portable-Executable-Leitfaden](guia-ejecutable-portable.md) → Firewall-Verwaltung, `resource_path()` und PyInstaller-Packaging.
- [📘 Neuen Knoten hinzufügen](adding-a-new-node.md) → Wie man `instrument/` erweitert und benutzerdefinierte Treiber registriert.
- [📄 `.sflow`-Format](sflow-format.md) → Wie Hardware-Konfigurationen und erfasste Arrays persistiert werden.
