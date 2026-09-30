---
title: FloWorks
description: Universelles visuelles Labor für Signalverarbeitung, wissenschaftliche Instrumentierung und Automatisierung.
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## Universelles visuelles Labor für Signale, Instrumentierung und KI

Wissenschaftliche Verarbeitung • DSP • VISA/SCPI • Automatisierung • Machine Learning

![FloWorks-Screenshot](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[Erste Schritte mit FloWorks](getting-started.md){ .md-button }
[Anatomie der Oberfläche](interface-anatomy.md){ .md-button .md-button--primary }
[Philosophie](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## Was ist FloWorks?
FloWorks ist ein **visuelles Open-Source-Labor** (Python + PySide6), in dem Sie Systeme aufbauen, indem Sie Blöcke (Knoten) verbinden, anstatt Codezeilen zu schreiben.

Stellen Sie sich eine digitale Zeichenfläche vor, auf der Sie Signalgeneratoren, mathematische Filter, Hardware-Controller (VISA/SCPI) und Modelle der Künstlichen Intelligenz mit virtuellen Kabeln verbinden. Alles basiert auf dem **Datenfluss**: Sie verbinden den Ausgang eines Blocks mit dem Eingang eines anderen, um Informationen zu verarbeiten, Geräte zu automatisieren oder Ergebnisse in Echtzeit zu analysieren.

Es richtet sich an Studenten, Forscher, Ingenieure und alle, die intuitiv experimentieren, lernen oder komplexe Systeme prototypisieren möchten, ohne die Barriere der traditionellen Programmierung.

### Mission
Die Zentralisierung des experimentellen Arbeitsablaufs in einem einzigen visuellen, offenen und zugänglichen Werkzeug. Wir möchten, dass sich die Benutzer auf *Experimentieren und Entdecken* konzentrieren, nicht auf den Kampf gegen die Komplexität der Software oder die Kosten der Lizenzen.

### Vision
Eine Welt, in der die einzige Barriere zwischen einer experimentellen Idee und ihrer Ausführung die Neugier des Experimentators ist. FloWorks strebt danach, die Referenzplattform für Wissenschaft und Technik zu sein, gebaut von und für die globale Gemeinschaft, und beseitigt die Mauern proprietärer Werkzeuge.

### Prinzipien
* **Absolute Freiheit:** Wissen und Werkzeuge müssen für jeden zugänglich sein. FloWorks ist kostenlos nutzbar und verpflichtet sich zu einem offenen und erweiterbaren Kern.
* **Unendliche Erweiterbarkeit:** Wenn ein Block fehlt, kann jeder ihn erstellen und mit Python in das Ökosystem integrieren.
* **Visuelle Transparenz:** Jeder Schritt des Prozesses kann grafisch inspiziert, debuggt und verstanden werden.
* **Verbindung mit der realen Welt:** Es ist nicht nur Simulation; es ermöglicht die Steuerung echter wissenschaftlicher Instrumente direkt von der Zeichenfläche aus.

Im Gegensatz zu geschlossenen oder hochspezialisierten Werkzeugen ist FloWorks als modulares, erweiterbares Ökosystem konzipiert, in dem jede Komponente ein wiederverwendbarer und verbindbarer Knoten ist.

---

## Hauptfunktionen

<div class="grid cards" markdown>

-   **:material-puzzle-outline: Erweiterbares Knoten-Ökosystem**

    Technischer Katalog in Schichten organisiert: Quellen, Verarbeitung, Steuerung, Hardware und Scripting.

    Dynamische Registrierung, deklarative Serialisierung und klare Verträge für schnelle Entwicklung.

    [:material-arrow-right: Knotenreferenz](node-reference.md)

-   **:material-connection: VISA/SCPI-Integration**

    Direkte Verbindung mit Oszilloskopen, LCR-Messgeräten und Generatoren.

    Multikanal-Unterstützung, integrierte Simulation via `PyVISA-py` und Firewall-Verwaltung im portablen Modus.

    [:material-arrow-right: Instrumentierung](instrumentation.md)

-   **:material-package-variant-closed: Portables `.sflow`-Format**

    Eigenständiger ZIP-Standard mit JSON-Graph, `.npy`-Arrays und Metadaten.

    Vollständige Reproduzierbarkeit von Experimenten und automatische DPI-Normalisierung.

    [:material-arrow-right: .sflow-Format](sflow-format.md)

-   **:material-translate: Erweiterte Internationalisierung**

    Sprachwechsel im laufenden Betrieb ohne Neustart der App.

    Hierarchische JSON-Übersetzungen und Persistenz von Einstellungen.

    [:material-arrow-right: i18n-Leitfaden](translation-guide.md)

-   **:material-tools: SDK und schnelle Entwicklung**

    Basisvorlage (`template_node.py`), Serialisierungs-Mixin und Schritt-für-Schritt-Anleitungen.

    Architektur vorbereitet für Plugins und Community-Erweiterung.

    [:material-arrow-right: Knoten erstellen](adding-a-new-node.md)

</div>

---

## Anwendungsbereiche

| Bereich | Anwendungen |
|------|--------------|
| 🎓 **Bildung** | Physik, Elektronik, Mathematik, STEM-Labore |
| ⚙️ **Ingenieurwesen** | DSP, Steuerung, Instrumentierung, Metrologie |
| 🤖 **KI** | ML, Optimierung, hybride Pipelines |
| 🔬 **Forschung** | Automatisierung und Datenerfassung |
| 🔌 **Hardware** | VISA/SCPI, Simulation und hybride Systeme |

---

!!! tip "Neu bei FloWorks?"

    Beginnen Sie mit dem Abschnitt **Erste Schritte mit FloWorks**, dann **Anatomie der Oberfläche**, um die Architektur der grafischen Benutzeroberfläche zu verstehen, und erkunden Sie schließlich die **Allgemeine Architektur**, um den Datenfluss und die Struktur des topologischen Motors zu verstehen.

---

<div markdown="1" style="text-align: center;">

## FloWorks

Visuelle Verarbeitung • Instrumentierung • Wissenschaft • KI

<small>Dokumentation erstellt mit MkDocs Material</small>

</div>
