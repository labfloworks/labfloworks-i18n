---
title: Erste Schritte mit FloWorks
description: Kurzanleitung zur Einrichtung der Umgebung, Ausführung Ihres ersten Flusses und Zugriff auf die portable Version.
---

# 🚀 Erste Schritte mit FloWorks

Diese Anleitung führt Sie von Null bis zur Ausführung Ihres ersten Signalverarbeitungsflusses. FloWorks ist eine Flussdiagramm-Anwendung für Signale, die mit Python und PySide6 erstellt wurde und echte Hardware (VISA/SCPI), integrierte Simulation, erweitertes Scripting und Hot-Language-Switching unterstützt.

---

## 🌊 Ihr erster Beispielfluss

Lassen Sie uns einen einfachen Fluss erstellen: ein sinusförmiges Signal erzeugen und in Echtzeit visualisieren.

1. **Knoten hinzufügen**
   Wählen Sie in der oberen Symbolleiste `Quelle` → wählen Sie `Erweiterter Signalgenerator`. Dann unter `Verarbeitung` → wählen Sie beispielsweise `Spektral`.
2. **Verbinden**
   Klicken Sie mit `Ctrl+Klick` auf den Ausgangsport (`rechts`) des Generators. Dann klicken Sie auf den Eingangsport (`links`) des Oszilloskops. Oder klicken Sie einfach auf den Ausgangsport und ziehen (halten Sie die Klick-Taste gedrückt) bis zum Eingangsport des nächsten Knotens.
3. **Konfigurieren (optional)**
   Klicken Sie auf einen Knoten, im linken Seitenpanel erscheint ein Editor mit den Parametern des ausgewählten Knotens, um die Betriebsbedingungen anzupassen. Im unteren Bereich befindet sich ein Visualisierungsdiagramm, das die von den Knoten erzeugten oder erfassten Daten visuell darstellt.
4. **Ausführen**
   Drücken Sie `F5` oder die Schaltfläche ▶ in der Symbolleiste. Die Topologie-Engine berechnet die Ausführungsreihenfolge, verarbeitet die Daten und Sie sehen die Welle im Grafikpanel. Der Konnektor wird animiert und zeigt den aktiven Fluss an!

---

## 🧠 Ports verstehen: Kategorien nach Farbe

In FloWorks gehört jeder Port zu einer **funktionalen Kategorie**, die durch eine Farbe identifiziert wird. Gültige Verbindungen werden **immer zwischen Ports derselben Farbe** hergestellt: ein Ausgang einer Kategorie verbindet sich nur mit einem Eingang derselben Kategorie. Außerdem übernimmt die Verbindungslinie automatisch die Farbe der verbundenen Ports, was die visuelle Lesbarkeit erleichtert.

| Typ | Farbe | Zweck | Typisches Beispiel |
|------|-------|-----------|----------------|
| `control` | Weiß | Steuerfluss / Aktivierung. | Startsignal zu einem Erfassungsknoten. |
| `exec` | Grau | Ausführung von Operationen oder Schritten. | Auslösung einer Funktion oder eines Callbacks. |
| `data` | Grün | Generische Daten / numerische Signale. | Ausgang eines Generators oder Sensors. |
| `int` | Blau | Ganze Zahlen. | Index, Puffergröße, ID. |
| `float` | Cyan | Gleitkommazahlen. | Amplitude, Frequenz, Schwellenwert. |
| `string` | Lila | Textzeichenketten. | Dateiname, Beschriftung. |
| `bool` | Rosa | Boolesche Werte (`True`/`False`). | Statusflag, Aktivierung. |
| `array` | Dunkelblau | Arrays / Vektoren. | Mehrkanalsignal, Liste von Abtastwerten. |
| `trigger` | Orange | Trigger / diskrete Ereignisse. | Synchronisationspuls, Flanke. |

**Goldene Regel:**

- Es werden nur Ports der **exakt gleichen Farbe** verbunden (Ausgang ↔ Eingang derselben Kategorie).
- Das System verhindert ungültige Verbindungen und hebt kompatible Ports beim Ziehen visuell hervor.
- Die Verbindungslinie nimmt die Farbe der verbundenen Ports an; so ist jeder Pfad auf einen Blick erkennbar.

**FloWorks-Philosophie:**  
Datenports **bewahren die Dimensionalität** der Arrays. Es wird niemals eine automatische Abflachung angewendet: wenn eine Matrix eingeht, kommt eine Matrix heraus, wodurch die Integrität Ihrer mehrdimensionalen Signale erhalten bleibt.

![FloWorks](assets/tipos_de_puertos.PNG)

---

## 🖱️ Navigation im Canvas

Beherrschen Sie den Arbeitsbereich mit diesen Gesten:

| Aktion | Wie es geht |
|--------|--------------|
| **Zoom** | Mausrad oder `Ctrl + Mausrad` |
| **Panning (Verschieben)** | Halten Sie `Leertaste` und ziehen, oder verwenden Sie die mittlere Maustaste |
| **Knoten auswählen** | Linksklick auf den Knoten |
| **Mehrfachauswahl** | Ziehen Sie ein Rechteck mit Linksklick, oder `Ctrl + Klick` auf mehrere Knoten |
| **Auswahl verschieben** | Ziehen Sie einen der ausgewählten Knoten |
| **Konfiguration öffnen** | Doppelklick auf einen Knoten |

**Tipp:** Das linke Panel aktualisiert sich automatisch mit der Konfiguration des ausgewählten Knotens, ohne zusätzliche Fenster öffnen zu müssen.

---

## ⚡ Tastenkürzel und erweiterte Bewegungen

Diese Tastenkürzel verwandeln einen normalen Benutzer in einen **Power-User**:

| Tastenkürzel | Aktion |
|-------|--------|
| `F5` | Fluss ausführen |
| `Ctrl + S` | Projekt speichern (`.sflow`) |
| `Ctrl + Klick` | Knoten verbinden (Klick auf Ausgangsport → Klick auf Eingangsport) |
| `Ctrl + C` / `Ctrl + V` | Ausgewählte Knoten kopieren / einfügen |
| `Ctrl + Z` / `Ctrl + Y` | Rückgängig / Wiederherstellen |
| `Ctrl + Shift + L` | Knoten im Canvas automatisch anordnen |
| `Entf` | Ausgewählte Knoten löschen |
| `Ctrl + A` | Alle Knoten auswählen |

**Erweiterte Bewegungen:**

- **Einen Fluss duplizieren:** Wählen Sie eine Gruppe von Knoten aus, `Ctrl + C`, `Ctrl + V` und ziehen Sie die Kopie in einen anderen Bereich.
- **Gitter bereinigen:** Verwenden Sie `Ctrl + Shift + L`, um die gesamte Zeichenfläche mit einem einzigen Befehl zu ordnen.
- **Schnellverbindung:** `Ctrl + Klick` auf einen Ausgangsport und dann normal auf den Eingangsport; FloWorks zeichnet die Verbindung automatisch.

---

## 🎨 Umgebungsanpassung

FloWorks passt sich Ihnen an, nicht umgekehrt.

### Hot-Theme-Wechsel
Wählen Sie in der oberen Leiste im Menü **Ansicht → Design** zwischen hell, dunkel oder anderen. Die Benutzeroberfläche ändert sich **sofort**, ohne Neustart und ohne den Arbeitsablauf zu verlieren.

### Schriftgröße
Wählen Sie unter **Ansicht → Schriftgröße** einen vordefinierten oder benutzerdefinierten Wert. Die gesamte Benutzeroberfläche passt sich sofort an.

### Sprache
Wählen Sie unter **Ansicht → Sprache** die gewünschte Sprache aus. FloWorks unterstützt **Hot-Switching**: Menüs, Schaltflächen und Nachrichten werden übersetzt, ohne die Anwendung neu zu starten.

---

## ❗ Lösung häufiger Probleme

| Problem | Mögliche Ursache | Lösung |
|----------|---------------|----------|
| Der Fluss wird nicht ausgeführt | Es gibt nicht konfigurierte Knoten oder unterbrochene Verbindungen | Überprüfen Sie, ob alle Knoten gültige Parameter haben und die Verbindungen zwischen kompatiblen Ports sind |
| Das Diagramm wird nicht aktualisiert | Der Fluss ist pausiert oder es fließen keine Daten | Stellen Sie sicher, dass Sie `F5` oder ▶ gedrückt haben und dass die Quellknoten Daten erzeugen |
| Ich kann zwei Knoten nicht verbinden | Die Ports sind unterschiedlichen Typs | Überprüfen Sie, ob beide Ports **Daten** oder beide **Steuerung** sind |
| Das Programm ist bei großen Flüssen langsam | Zu viele Knoten oder Echtzeitdiagramme | Schließen Sie nicht verwendete Analysepanele oder reduzieren Sie die Abtastrate der Quellknoten |
| Das Design ändert sich nicht | Einige Widgets sind möglicherweise nicht registriert | Starten Sie die Anwendung neu und versuchen Sie es erneut (in zukünftigen Versionen behoben) |

---

## 🧪 Schnelle praktische Beispiele

Zusätzlich zum anfänglichen Sinusfluss probieren Sie diese Mini-Projekte aus, um FloWorks zu beherrschen:

| Beispiel | Beteiligte Knoten | Erwartetes Ergebnis |
|---------|-------------------|--------------------|
| **Tiefpassfilter** | Generator → Filter → Grafikvisualisierer | Sie sehen das gefilterte Signal |
| **Simulierte Erfassung** | Generator → THD-Analysator | Wert der harmonischen Verzerrung des Signals |
| **Manuelle Steuerung** | Generator → Dateninspektor | Tabelle mit den Werten des vom Generator gesendeten Signals |
| **Signalvergleich** | Zwei Generatoren → Addierer → Grafikvisualisierer | Das Ergebnis der Operation (Addition, Subtraktion, Multiplikation oder Division) von zwei Wellen in einem einzigen Diagramm |

Jeder dieser Flüsse kann in weniger als einer Minute aufgebaut werden, was die Agilität von FloWorks gegenüber herkömmlichem Code demonstriert.

---

## 📚 Was kommt als Nächstes?

| Ressource | Beschreibung |
|---------|-------------|
| [🗺️ Leitfaden zur Anatomie der Benutzeroberfläche](interface-anatomy.md) | Verständnis der Architektur und Philosophie der grafischen Benutzeroberfläche |
| [🗺️ Code-Karte und Architektur](philosophy.md) | Vollständige Struktur, Manager, Verträge und DPI-Awareness. |
| [🧩 Technische Knotenreferenz](node-reference.md) | Katalog, `ScriptNode`, Mehrkanal und wie man das System erweitert. |
| [🌐 Internationalisierungsleitfaden](translation-guide.md) | Sprachen hinzufügen, JSON validieren und `tr()`-Schlüssel verwalten. |
| [📦 Portable Build-Leitfaden](guia-ejecutable-portable.md) | PyInstaller, Hooks, `--onefile`, Fehlerbehebung und digitale Signatur. |

---

!!! warning "Hinweise zur Kompatibilität und Verwendung"
    1. **Python-Version:** Sie können 3.9+ und 64-Bit-Systeme verwenden.
    2. **Windows-Firewall:** Wenn Sie echte Hardware (VISA/SCPI-Oszilloskop) verwenden, erlauben Sie `FloWorks.exe` in der Firewall. Die App zeigt einen benutzerdefinierten Dialog an, wenn die Verbindung blockiert ist (der OS-Dialog erscheint nicht im `--windowed`-Modus).
    3. **Wichtige Tastenkürzel:** `F5` (ausführen), `Ctrl+S` (`.sflow` speichern), `Ctrl+Klick` (verbinden), `Leertaste+Klick` (freies Panning), `Ctrl+Shift+L` (Auto-Layout).
    4. **Datenerhaltung:** Die Engine wendet **niemals** `flatten()` auf Arrays an. Arbeiten Sie mit lokalen Kopien, wenn Sie vektorisieren müssen.
