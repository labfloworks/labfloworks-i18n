# Philosophie

## Übersicht

FloWorks ist eine Desktop-Anwendung, die es Ihnen ermöglicht, Signalverarbeitungsketten über visuelle Flussdiagramme zu erstellen.  
Ziehen, verbinden und konfigurieren Sie Knoten; das Ergebnis wird in Echtzeit berechnet und angezeigt.  
Arbeiten Sie mit simulierten Signalen oder verbinden Sie echte Geräte (Oszilloskope, Generatoren, LCR-Multimeter), ohne Code schreiben zu müssen, wobei Ihnen jedoch eine leistungsstarke Skriptumgebung zur Verfügung steht, falls Sie die Funktionalität erweitern möchten.

---

## Hauptmerkmale

- **Interaktive Diagramme** – Erstellen Sie Ihren Arbeitsablauf, indem Sie Knoten mit Linien verbinden, die den Datenfluss darstellen.
- **Echtzeitverarbeitung** – Jede Änderung wird sofort in den Diagrammen und Visualisierungen widergespiegelt.
- **Simulation und echte Hardware** – Erzeugen Sie Testsignale oder erfassen Sie Daten direkt von Laborgeräten.
- **Erweiterter Skriptknoten** – Integrieren Sie Ihren eigenen Python-Code mit Autovervollständigung, bearbeitbaren dynamischen Parametern und dauerhaftem Speicher zwischen Ausführungen.
- **Professionelle Visualisierung** – Signale, Spektren, Spektrogramme und hochwertige Diagramme, bereit zum Exportieren.
- **Mehrsprachig** – Die Benutzeroberfläche erkennt die Systemsprache und ermöglicht jederzeit das Umschalten zwischen Spanisch, Englisch und anderen Sprachen.
- **Visuelle Designs** – Dunkler, heller und Hochkontrastmodus, angepasst an Ihre Präferenzen oder Barrierefreiheitsanforderungen.
- **Vollständige Projektverwaltung** – Speichern Sie Ihre Arbeit in `.sflow`-Dateien und stellen Sie sie genau so wieder her, wie Sie sie verlassen haben, mit unbegrenztem Rückgängigmachen und Wiederherstellen.

---

## Wie man mit FloWorks arbeitet

### Knoten
Ein Knoten ist ein Baustein der Verarbeitung. Sie sind in drei Kategorien organisiert:

- **Quellen** – Fügen Signale am Anfang des Flusses ein. Zum Beispiel ein Oszilloskop (echt oder simuliert), ein Funktionsgenerator oder eine mathematische Operation.
- **Verarbeitung** – Transformiert die Daten. Additionen, Subtraktionen, Bedingungen, Filter… einschließlich eines speziellen Knotens zum Schreiben Ihrer eigenen Python-Skripte.
- **Senken** – Zeigen die Ergebnisse an oder exportieren sie. Die grafische Visualisierung und der professionelle Grafikexporteur sind die am häufigsten verwendeten.

### Verbindungen
Die Verbindungen zwischen Knoten werden als glatte Kurven oder orthogonale Linien gezeichnet. Eine Flussanimation zeigt jederzeit die Richtung der Daten an. Das System organisiert die Kabel automatisch, damit sie sich nicht überlappen.

### Visualisierung
Wann immer ein Knoten ein Signal erzeugt, kann es im integrierten Diagrammpanel angezeigt werden. Sie können verschiedene Darstellungen (Wellenform, Spektrum, Spektrogramm) erkunden und die Skalierung mit der Maus anpassen.

---

## Hervorgehobene Knoten
Dies sind die unverzichtbaren Mindestknoten, die notwendig sind, damit die Philosophie des Programms Sinn ergibt.

### Signalerzeugungsknoten
Eine Signalquelle, die benutzerdefinierte Simulationen von Wellenformen nach Belieben des Benutzers erzeugen kann. Ermöglicht über ein Kontextmenü die Auswahl oder Eingabe der gewünschten Wellenform.

### Skriptknoten
Eine vollständige Programmierumgebung innerhalb des Diagramms:

- **Editor mit Syntaxhervorhebung**, Autovervollständigung und Fehlerkonsole.
- **Dynamische Parameter** – Definieren Sie bearbeitbare Variablen über das Knotenpanel, ohne den Code zu ändern.
- **Konfigurierbare Ports** – Fügen Sie zusätzliche Eingänge und Ausgänge direkt aus dem Editor hinzu.
- **Dauerhafter Zustand** – Speichern Sie Werte zwischen Ausführungen; alles wird zusammen mit dem Projekt gespeichert.

### Grafikexporteur
Ein Senkenknoten, der hochwertige Bilder für Berichte oder Publikationen erzeugt. Ermöglicht die Konfiguration von Größe, Auflösung, Format und mehr.

---

## Anpassung

- **Sprache** – Die Anwendung erkennt automatisch die Systemsprache und speichert Ihre Präferenz. Sie können sie über das Menü ändern, ohne neu zu starten.
- **Erscheinungsbild** – Wählen Sie zwischen dunklem, hellem oder Hochkontrast-Design je nach Umgebungslicht oder Ihren visuellen Bedürfnissen.

---

## Projekte und Dateien

Speichern Sie Ihr vollständiges Diagramm in einer `.sflow`-Datei.  
Beim Öffnen stellen Sie alle Knoten, Verbindungen, Skripte, Parameter und Visualisierungseinstellungen wieder her.  
Rückgängig- und Wiederherstellen-Aktionen ermöglichen es Ihnen, ohne Angst vor Datenverlust zu experimentieren.

---

FloWorks ist so konzipiert, dass Sie sich auf die Signalanalyse konzentrieren können, nicht auf die technischen Details der Implementierung. Ziehen, verbinden und entdecken Sie.
