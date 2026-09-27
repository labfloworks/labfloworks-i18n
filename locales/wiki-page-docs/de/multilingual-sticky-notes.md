# Sticky Notes (Haftnotizen) – Benutzerhandbuch

## Was sind Haftnotizen?

Haftnotizen (oder *sticky notes*) sind kleine Textblöcke, die Sie frei auf dem Diagramm platzieren können. Sie dienen dazu:

- Erinnerungen, Titel oder Erklärungen direkt auf der Zeichenfläche hinzuzufügen.
- Schritt-für-Schritt-Tutorials zu erstellen, die den Benutzer Ihres Projekts anleiten.
- Teile des Arbeitsablaufs zu dokumentieren, ohne FloWorks zu verlassen.
- Kommentare für sich selbst oder andere Mitwirkende zu hinterlassen.

Die Notizen sind in der Größe veränderlich (durch Ziehen der Ecken), können an jeden beliebigen Ort des Diagramms verschoben und zusammen mit dem Projekt gespeichert werden. Beim Öffnen einer `.sflow`-Datei erscheinen alle Notizen genau dort, wo Sie sie hinterlassen haben.

---

## Die Neuheit: mehrsprachige Notizen

Haftnotizen können den Text automatisch in der Sprache anzeigen, die Sie für die Anwendung ausgewählt haben.  
Anstatt die endgültige Nachricht in einer einzigen Sprache zu verfassen, können Sie **spezielle Marker** einfügen, die sich beim Ändern der Sprache in FloWorks automatisch übersetzen.

So kann dieselbe Notiz auf Spanisch, Englisch oder jeder anderen verfügbaren Sprache gelesen werden, ohne den Text jedes Mal bearbeiten zu müssen.

---

## So schreiben Sie eine mehrsprachige Notiz

Innerhalb einer Notiz (erstellen Sie eine per Doppelklick oder mit der Schaltfläche 📝 in der Werkzeugleiste) können Sie zwei Arten von Markern verwenden:

### 1. Mit dem Wort `tr(…)`
Schreiben Sie `tr("schlüssel")` und ersetzen Sie `schlüssel` durch einen beschreibenden Namen für die Phrase.

Beispiel:

```
tr("tutorial.paso1.titulo")
tr("tutorial.paso1.mensaje")
```

### 2. Mit doppelten geschweiften Klammern `{{…}}`
Schreiben Sie `{{schlüssel}}` auf dieselbe Weise.

Beispiel:

```
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Beide Formate funktionieren gleich; wählen Sie das, das Ihnen angenehmer ist (Sie können sie sogar in derselben Notiz kombinieren).

> **Wichtig**: Der Text, den Sie beim Bearbeiten der Notiz sehen, enthält die ursprünglichen Marker (z. B. `{{tutorial.paso1.titulo}}`).  
> Wenn Sie mit dem Bearbeiten fertig sind und zur normalen Diagrammansicht zurückkehren, werden die Marker durch die in die aktuelle Sprache der Anwendung übersetzte Phrase ersetzt.

---

## Verhalten beim Sprachwechsel

- Wenn Sie die Sprache über das FloWorks-Menü ändern (z. B. von Spanisch auf Englisch), **werden alle Haftnotizen, die Marker enthalten, automatisch aktualisiert**.
- Es ist nicht nötig, das Projekt zu schließen und erneut zu öffnen oder jede Notiz manuell zu berühren.
- Notizen, die nur normalen Text (ohne Marker) enthalten, sind nicht betroffen; sie zeigen in jeder Sprache dasselbe an.

---

## Vorteile der Verwendung von Markern

- **Sofortige mehrsprachige Tutorials** – Dieselbe Notiz dient als Anleitung für Benutzer verschiedener Sprachen.
- **Konsistenz** – Wenn Sie die Übersetzung an einer einzigen Stelle ändern (in der Sprachdatei, die Ihr Entwicklungsteam pflegt), werden alle Notizen, die diesen Schlüssel verwenden, aktualisiert.
- **Einfache Wartung** – Sie können den Inhalt einmal schreiben und in mehreren Notizen wiederverwenden.
- **Flexibilität** – Kombinieren Sie festen Text mit Markern. Zum Beispiel:

```
🎯 SCHRITT 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

---

## Praktisches Beispiel: ein Schritt-für-Schritt-Tutorial

Angenommen, Sie möchten eine Notiz hinzufügen, die den ersten Schritt eines Tutorials erklärt.  
Im Bearbeitungsmodus schreiben Sie:

```
🎯 SCHRITT 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Wenn Sie mit dem Bearbeiten fertig sind und die Anwendung auf Spanisch verwenden, sehen Sie:

```
🎯 PASO 1
¡Bienvenido a FloWorks!
Arrastre un nodo fuente de señal para comenzar.
```

Wenn Sie die Sprache auf Englisch ändern, zeigt dieselbe Notiz:

```
🎯 STEP 1
Welcome to FloWorks!
Drag a signal source node to begin.
```

Und so weiter für jede andere konfigurierte Sprache.

---

## Zusammenfassung

- Haftnotizen bereichern Ihre Diagramme mit Textinformationen.
- Sie können jetzt **mehrsprachig** sein, indem Sie die Marker `tr("schlüssel")` oder `{{schlüssel}}` verwenden.
- Beim Bearbeiten sehen Sie die Schlüssel; in der Ansicht den übersetzten Text.
- Ändern Sie die Sprache der Anwendung und alle Notizen passen sich sofort an.
- Ideal für die Erstellung von visueller Dokumentation, Tutorials oder Hinweisen, die in mehreren Sprachen funktionieren sollen.

Nutzen Sie diese Funktion, um Ihre Projekte zugänglicher und einfacher teilbar zu machen!
