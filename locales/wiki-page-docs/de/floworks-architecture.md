---
title: Architektur von FloWorks
description: Übersicht der Komponenten und internen Funktionsweise für den Endbenutzer
---

# Architektur von FloWorks – Ansicht für den Benutzer

FloWorks ist eine Desktop-Anwendung, die es Ihnen ermglicht, Signalverarbeitungsketten über Flussdiagramme zu erstellen. Sie verbindet Blöcke (Knoten) auf einer interaktiven Zeichenfläche und zeigt die Ergebnisse in Echtzeit an. Um dies zu ermöglichen, ist die Anwendung in mehrere Module organisiert, die zusammenarbeiten. Nachfolgend wird, ohne technische Details, erklärt, was jeder Teil tut und wie sie zusammenhängen.

---

## Allgemeine Struktur

Die Anwendung besteht aus den folgenden Funktionsbereichen:

| Bereich | Was macht er? |
|---------|---------------|
| **Start und Hauptfenster** | Startet das Programm, zeigt das Fenster, die Menüs an und koordiniert alle Benutzeraktionen. |
| **Ausführungsengine** | Berechnet die Reihenfolge, in der die Knoten ausgeführt werden müssen, erkennt Abhängigkeiten und Zyklen und überträgt die Daten von einem Knoten zum anderen. |
| **Szene und Diagramm** | Verwaltet die Zeichenfläche, auf der Sie die Knoten platzieren, die Verbindungen zwischen ihnen, die Haftnotizen und die Rückgängig-/Wiederherstellen-Aktionen. |
| **Knoten und Verarbeitung** | Enthält alle Blocktypen, die Sie verwenden können: Signalquellen, mathematische Operationen, benutzerdefinierte Skripte, Grafikexport usw. |
| **Visuelle Verbinder** | Zeichnet die Linien, die die Knoten verbinden (glatte Kurven oder orthogonale Pfade), animiert sie, um den Datenfluss anzuzeigen, und verhindert Überlappungen. |
| **Benutzeroberfläche** | Umfasst die Diagrammansicht (Zoom, Verschiebung), die Symbolleiste, die Parametertabelle, die Analysepanele (Statistiken, Cursor) und die Konfigurationsdialoge. |
| **Unterstützung für echte Hardware** | Ermöglicht die Kommunikation mit Laborgeräten (Oszilloskope, Generatoren, LCR-Multimeter) zum Erfassen oder Erzeugen echter Signale. |
| **Grafikexport** | Erzeugt qualitativ hochwertige Bilder (PNG, PDF, SVG) mit vollständiger visueller Anpassung. |
| **Designs und Erscheinungsbild** | Ändert das Erscheinungsbild der gesamten Anwendung (dunkel, hell, hoher Kontrast) und ermöglicht die Anpassung der Schriftgröße. |
| **Sprachen** | Übersetzt die gesamte Benutzeroberfläche in mehrere Sprachen und ermöglicht sofortige Sprachwechsel. |
| **Projektverwaltung** | Speichert und öffnet `.sflow`-Dateien mit dem gesamten Diagramm, einschließlich Konfigurationen, Skripten und Ergebnissen. |
| **Tests und Diagnose** | Interne Werkzeuge zur Überprüfung der korrekten Funktionsweise (nicht sichtbar für den Endbenutzer). |

---

## Wie es intern funktioniert

### Start und Hauptfenster
Beim Öffnen von FloWorks wird die grafische Umgebung konfiguriert, die Pixeldichte Ihres Bildschirms erkannt (damit alles auf 4K- und normalen Monitoren scharf aussieht) und das Hauptfenster angezeigt. Dieses Fenster bündelt alle Elemente: den Zeichenbereich, die Menüs, die Symbolleiste und die Seitenpaneele.

### Fluss-Engine
Wenn Sie "Ausführen" drücken (oder F5), durchläuft eine interne Engine alle Knoten in der richtigen Reihenfolge und beachtet dabei die Verbindungen. Sie weiß, welche Knoten von anderen abhängen, und verhindert Endlosschleifen. Sie unterstützt, dass ein Knoten mehrere benannte Eingaben erhält und mehrere Ausgaben produziert. Die Daten wandern zwischen den Knoten, ohne ihre ursprüngliche Struktur zu verlieren.

### Diagrammszene
Die Zeichenfläche, auf der Sie Ihre Diagramme erstellen, ist eine intelligente Szene:

- Ermöglicht das Hinzufügen, Verschieben, Verbinden und Auswählen von Knoten.
- Unterstützt unbegrenztes Rückgängigmachen und Wiederherstellen für jede Aktion.
- Enthält in der Größe veränderbare Haftnotizen, die Sie frei platzieren können und die mit dem Projekt gespeichert werden.
- Verfügt über eine automatische Anordnung, die die Knoten ordentlich neu positioniert (mit Strg+Shift+L).
- Beim Speichern wird das gesamte Diagramm in eine `.sflow`-Datei gepackt, die die Knotenbeschreibungen, Verbindungen, Notizen und die zugehörigen numerischen Daten enthält.

### Verbinder
Die Linien, die die Knoten verbinden, werden als glatte Kurven oder orthogonale Pfade gezeichnet. Eine sanfte Animation von Punkten oder Strichen zeigt die Flussrichtung an. Ein Spurmanager verhindert, dass sich mehrere Verbindungen zwischen denselben Knoten stapeln; er trennt sie automatisch, damit alles lesbar bleibt.

### Knotentypen
Die Knoten sind die Grundbausteine. Sie sind in drei Kategorien gruppiert:

- **Quellen** – Erzeugen Signale. Sie können Wellen simulieren (Sinus, Rechteck usw.) oder echte Daten von einem angeschlossenen Oszilloskop oder Multimeter lesen. Sie unterstützen mehrere Kanäle gleichzeitig (z. B. Impedanz und Phase von einem LCR).
- **Verarbeitung** – Transformieren die Daten. Sie umfassen arithmetische Operationen (Addition, Subtraktion, Multiplikation, Division), bedingte Entscheidungen (Ja/Nein-Verzweigung) und einen leistungsstarken Skriptknoten, der es Ihnen ermöglicht, Ihren eigenen Python-Code mit visuellen Hilfen zu schreiben.
- **Senken** – Zeigen die Ergebnisse an oder exportieren sie. Der gebräuchlichste ist der Grafik-Visualizer (virtuelles Oszilloskop), es gibt aber auch einen professionellen Grafikexporteur.

Jeder Knoten hat Eingabe-Ports (links/oben) und Ausgabe-Ports (rechts/unten). Wenn Sie einen Ausgabe-Port mit einem Eingabe-Port verbinden, fließt das Signal zwischen ihnen.

#### Erweiterter Skriptknoten
Der Skriptknoten verdient besondere Erwähnung. Er ist für fortgeschrittene Benutzer gedacht, die ihre eigene Verarbeitung hinzufügen möchten, ohne FloWorks zu verlassen. Er bietet:

- Einen Editor mit Syntaxhervorhebung, Autovervollständigung und Zeilennummern.
- Die Möglichkeit, bearbeitbare Parameter über das Knotenpanel zu definieren, ohne den Code zu berühren (z. B. ein numerischer Wert, der später im Skript verwendet wird).
- Dynamische Ein- und Ausgabe-Ports: Durch das Hinzufügen spezieller Kommentare im Skript können Sie neue Verbinder erstellen.
- Dauerhaften Speicher: Eine spezielle Variable (`persist`), die ihren Wert zwischen Ausführungen behält, nützlich für Akkumulatoren oder Zustandsautomaten.
- Bereits vorbereitete Skriptvorlagen und die Möglichkeit, eigene zu speichern.
- Ein integriertes Hilfesystem und eine Konsole, die Ausführungsfehler anzeigt.

### Benutzeroberfläche
Neben der Zeichenfläche umfasst die Benutzeroberfläche:

- Eine **Symbolleiste** mit allen nach Kategorien organisierten Knoten, Menüs für Sprache, Design und Schriftgröße sowie Zugriff auf die Protokollanzeige.
- Eine **Parametertabelle**, die Informationen über die ausgewählten Knoten anzeigt und mögliche Inkompabilitäten hervorhebt (z. B. der Versuch, mit Signalen unterschiedlicher Länge zu arbeiten).
- **Andockbare Analysepanele**: Statistiken (Maximum, Minimum, Effektivwert), A/B-Cursor zum Messen von Differenzen und ein Fadenkreuz mit Peak-Marker.
- Einen **Willkommensdialog**, der sich an die Auflösung Ihres Bildschirms anpasst und Ihnen anfängliche Optionen bietet.

### Verbindung mit echten Geräten
Wenn Sie kompatible Hardware haben (Siglent SDS Oszilloskope, LCR-Multimeter, SDG-Generatoren), kann FloWorks über das Standardprotokoll VISA/SCPI mit ihnen kommunizieren. Die Konfiguration erfolgt über spezifische Paneele innerhalb der Anwendung. Wenn Sie ein Multikanal-Signal erfassen (z. B. Betrag und Phase von einem LCR), packt der Quellenknoten alle Kanäle zusammen, und Sie können über ein einfaches Kontextmenü auswählen, welchen Sie visualisieren möchten.

### Professioneller Grafikexport
Der Grafikexporteur ermöglicht es Ihnen, bilder zu erzeugen, die für Berichte oder Veröffentlichungen bereit sind. Ein Doppelklick darauf öffnet einen Dialog mit mehreren Optionen: Sie können Farben, Linientypen, Beschriftungen, Skalen anpassen, zwischen PNG-, PDF- oder SVG-Format wählen und Ihre Einstellungen als wiederverwendbare Profile speichern.

### Visuelle Anpassung
FloWorks enthält mehrere Designs (dunkel, hell, hoher Kontrast), die das Erscheinungsbild der gesamten Anwendung sofort ändern, ohne Neustart. Außerdem können Sie die globale Schriftgröße über das Menü anpassen (Informationen → Schriftgröße), und alle Elemente werden entsprechend neu skaliert, einschließlich der Texte innerhalb der Knoten, der Haftnotizen und der Grafiken.

### Sprachsystem
Die Anwendung erkennt beim ersten Start automatisch die Systemsprache und speichert Ihre Präferenz. Sie können die Sprache jederzeit über das Menü ändern; alle Texte, Menüs und Hilfen werden sofort aktualisiert.

### Projekte und `.sflow`-Dateien
Ihre gesamte Arbeit wird in einer einzigen Datei mit der Erweiterung `.sflow` gespeichert. Diese Datei enthält das vollständige Diagramm: Knoten, Verbindungen, Notizen, Konfigurationen, Skripte und die erzeugten numerischen Daten. Sie können sie mit anderen Benutzern teilen; beim Öffnen auf einem anderen Computer werden die Notizen und Knoten automatisch an die Pixeldichte dieses Bildschirms angepasst.

---

## Typische Arbeitsabläufe

1. **Ein einfaches Diagramm erstellen**  
   Wählen Sie einen Quellenknoten (z. B. Generator) und einen Visualisiererknoten aus der Symbolleiste.  
   Verbinden Sie den Ausgang des Generators mit dem Eingang des Visualisierers (Strg+Klick auf den Ausgabe-Port, dann Klick auf den Eingabe-Port).  
   Drücken Sie F5 zum Ausführen. Sie werden das Signal im Diagramm sehen.

2. **Ein benutzerdefiniertes Skript verwenden**  
   Fügen Sie einen Skriptknoten hinzu.  
   Schreiben Sie Ihren Python-Code im Editor; Sie können bearbeitbare Parameter und zusätzliche Ports definieren.  
   Verbinden Sie seine Ein- und Ausgänge wie bei jedem anderen Knoten.  
   Führen Sie den Fluss aus; das Skript wird mit Ihren Daten verarbeitet.

3. **Daten von einem echten Oszilloskop erfassen**  
   Schließen Sie das Gerät an und konfigurieren Sie die Kommunikation über das Panel des Oszilloskop-Knotens.  
   Der Knoten erfasst das Signal und gibt es über seine Ausgabe-Ports weiter (einer pro Kanal).  
   Verbinden Sie diese Ports mit anderen Verarbeitungsknoten oder dem Visualisierer.

4. **Ein Diagramm für einen Bericht exportieren**  
   Verbinden Sie das gewünschte Signal mit einem Grafikexporteur-Knoten.  
   Wählen Sie im Knoten (Rechtsklick), um das visuelle Erscheinungsbild des Diagramms zu konfigurieren.  
   Profile können auch geladen/gespeichert werden, um die Erstellung berichtsfertiger Grafiken in dem gewählten Format zu beschleunigen.

---

## Wozu das alles dient

Diese Architektur ist so konzipiert, dass Sie sich auf die Signalanalyse konzentrieren können, ohne sich Gedanken über die interne Organisation des Programms machen zu müssen. Jede Komponente hat eine klare Funktion und arbeitet zusammen, um ein reibungsloses Erlebnis zu bieten, von der Simulation bis zur realen Instrumentierung, einschließlich visueller Anpassung und Ergebnisexport.

Sollten Sie jemals die Fähigkeiten von FloWorks erweitern müssen (z. B. durch Hinzufügen neuer Knotentypen oder Anschließen eines anderen Geräts), wissen Sie, dass eine modulare Struktur dies ermöglicht, auch wenn dies Entwicklerthemen sind. Als Endbenutzer können Sie die Flexibilität genießen, die dieses Design bietet.
