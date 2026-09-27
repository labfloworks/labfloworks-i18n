---
title: Leitfaden zum Hinzufügen eines neuen Knotens zu FloWorks
description: Schritt-für-Schritt-Tutorial zum Erstellen, Registrieren und Integrieren benutzerdefinierter Knoten in die Flow-Engine, die Benutzeroberfläche, die visuellen Themen und das Internationalisierungssystem von FloWorks.
---

# 📘 Leitfaden für Entwickler: So fügen Sie FloWorks einen neuen Knoten hinzu

Dieser Leitfaden beschreibt den vollständigen Prozess zur Erstellung eines neuen Knotentyps in FloWorks und stellt sicher, dass er korrekt in die Flow-Engine, die Benutzeroberfläche, die visuellen Themen und das Internationalisierungssystem integriert wird.

---

## 📋 Inhaltsverzeichnis
- [📘 Leitfaden für Entwickler: So fügen Sie FloWorks einen neuen Knoten hinzu](#-leitfaden-für-entwickler-so-fügen-sie-floworks-einen-neuen-knoten-hinzu)
  - [📋 Inhaltsverzeichnis](#-inhaltsverzeichnis)
  - [1. Einführung in die Architektur](#1-einführung-in-die-architektur)
  - [2. Verwendung der Vorlage `template_node.py`](#2-verwendung-der-vorlage-template_nodepy)
  - [3. Schritt für Schritt: Erstellung eines benutzerdefinierten Knotens](#3-schritt-für-schritt-erstellung-eines-benutzerdefinierten-knotens)
    - [3.1. Vorlage kopieren und umbenennen](#31-vorlage-kopieren-und-umbenennen)
    - [3.2. Ports und Beschriftungen definieren](#32-ports-und-beschriftungen-definieren)
    - [3.3. Verarbeitungslogik implementieren](#33-verarbeitungslogik-implementieren)
    - [3.4. Erscheinungsbild anpassen (optional)](#34-erscheinungsbild-anpassen-optional)
    - [3.5. Konfigurierbare Parameter hinzufügen (optional)](#35-konfigurierbare-parameter-hinzufügen-optional)
    - [3.6. Knoten serialisierbar machen (Konfigurationen speichern / laden)](#36-knoten-serialisierbar-machen-konfigurationen-speichern--laden)
  - [4. Integration in das System](#4-integration-in-das-system)
  - [5. Internationalisierung (i18n)](#5-internationalisierung-i18n)
  - [6. Visuelle Themen](#6-visuelle-themen)
  - [7. Checkliste und Fehlerbehebung](#7-checkliste-und-fehlerbehebung)
    - [✅ Checkliste](#-checkliste)
    - [🐛 Häufige Probleme](#-häufige-probleme)
  - [8. Fazit](#8-fazit)

---

## 1. Einführung in die Architektur

FloWorks ist auf PySide6 aufgebaut und verwendet ein Modell verbindbarer Knoten, die einen Signalverarbeitungsfluss darstellen.

---

## 2. Verwendung der Vorlage `template_node.py`

Um die Erstellung neuer Knoten zu erleichtern, wird die Datei `nodes/template_node.py` bereitgestellt. Diese Vorlage enthält:
- Vollständige Unterstützung für Internationalisierung (Verbindung zu `languageChanged`, Methode `update_language`).
- Vollständige Unterstützung für Themen (Methode `update_theme`).
- Integrierte Hilfe im HTML-Format mit drei Abschnitten.
- Verwaltung mehrerer konfigurierbarer Ein-/Ausgabe-Ports.
- Mehrere Ausgaben mit `get_output_for_port`.
- Visualisierung im Plot über `get_display_signal`.
- Übersetzbares Kontextmenü.

Es wird empfohlen, bei der Entwicklung eines neuen Knotens immer von dieser Vorlage auszugehen.

---

## 3. Schritt für Schritt: Erstellung eines benutzerdefinierten Knotens

### 3.1. Vorlage kopieren und umbenennen
1. Kopieren Sie `nodes/template_node.py` unter dem Namen Ihres neuen Knotens, z. B. `nodes/mi_nodo.py`.
2. Benennen Sie die Klasse von `TemplateNode` in einen beschreibenden Namen um, z. B. `MiNodoNode`.
3. Passen Sie bei Bedarf die Imports an.

### 3.2. Ports und Beschriftungen definieren
!!! warning "Wichtig: Namensübereinstimmung"
    Die Portnamen in `PORTS`, `PORT_LABELS` und die Schlüssel des von `execute_program` zurückgegebenen Wörterbuchs müssen **exakt übereinstimmen** (einschließlich Groß-/Kleinschreibung). Die Vorlage enthält jetzt eine Alias-Zuordnung (`'data_in'` → erster linker Port) für mehr Robustheit.

Bearbeiten Sie das Wörterbuch `PORTS` am Anfang der Datei. Jeder Eintrag hat das Format:
```python
"port_name": ("seite", anteil)
```
- **Mögliche Seiten:** `"left"`, `"right"`, `"top"`, `"bottom"`.
- **Anteil:** Wert zwischen `0.0` und `1.0`, der die Position entlang der Seite angibt.

**Beispiel für einen Knoten mit einem Eingang und zwei Ausgängen:**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
Das Wörterbuch `PORT_LABELS` enthält den Text, der neben jedem Port angezeigt wird. Es wird empfohlen, Übersetzungsschlüssel anstelle von festem Text zu verwenden (siehe Abschnitt Internationalisierung).

### 3.3. Verarbeitungslogik implementieren
Die Schlüsselmethode ist `execute_program(self, input_data)`. Diese Methode wird von der Flow-Engine aufgerufen, wenn der Knoten Daten empfängt.

**`input_data` kann sein:**
- `None`, wenn kein Eingang vorhanden ist.
- Ein Tupel `(x, y)` für Zeitsignale.
- Ein 1D-Array.
- Ein Wörterbuch `{port_name: daten}` bei Knoten mit mehreren Eingängen.

**Rückgabewert:**
- Bei Knoten mit einer einzigen Ausgabe geben Sie die Daten direkt zurück (z. B. Tupel `(x, y)`).
- Bei Knoten mit mehreren Ausgängen geben Sie ein Wörterbuch zurück, dessen Schlüssel mit den in `PORTS` definierten Ausgabe-Portnamen übereinstimmen.

```python
def execute_program(self, input_data):
    # input_data verarbeiten und Ergebnisse erzeugen
    resultado_magnitud = (freq, mag)
    resultado_fase = (freq, phase)
    return {
        "magnitude": resultado_magnitud,
        "phase": resultado_fase
    }
```

!!! tip "Hinweis zu generischen Portnamen"
    Die Flow-Engine kann gelegentlich ein Wörterbuch mit Schlüsseln wie `'data_in'` anstelle des tatsächlichen Portnamens übergeben (besonders wenn der Benutzer nicht exakt auf den Kreis geklickt hat). Die Vorlage enthält bereits Code, um diesen Fall zu behandeln:
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    Dadurch wird verhindert, dass der Knoten aufgrund einer ungenauen Verbindung fehlschlägt.

Die Vorlage enthält bereits ein auskommentiertes Beispiel. Implementieren Sie außerdem `get_output_for_port(self, port_name)`, damit die Engine jede Ausgabe routen kann:
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. Erscheinungsbild anpassen (optional)
Die Methode `paint()` zeichnet den Hintergrund, den Titel, den Status und zusätzlichen Text. Sie können ändern:
- Die Farben (werden automatisch über `update_theme` aktualisiert).
- Den Statustext (unter Verwendung des Attributs `self._status`).
- Zusammenfassungsinformationen (z. B. Magnitudenspitze).

Die Vorlage zeigt ein einfaches Beispiel.

### 3.5. Konfigurierbare Parameter hinzügen (optional)
Wenn Ihr Knoten vom Benutzer anpassbare Parameter erfordert (z. B. Fenstergröße, Grenzfrequenz), können Sie:
1. Attribute in `__init__` hinzufügen (z. B. `self.window_size = 512`).
2. Ein Konfigurationsdialog erstellen (erben von `QDialog`).
3. Das Dialog in `open_config_dialog()` verbinden (Methode bereits in der Vorlage vorhanden).
4. Die Parameter aus dem Dialog aktualisieren und `self.update()` aufrufen.

### 3.6. Knoten serialisierbar machen (Konfigurationen speichern / laden)
Damit der Knoten seine Parameter beim Kopieren/Einfügen, Rückgängig/Wiederholen oder bei Verwendung der Befehle Speichern/Öffnen im Datei-Menü speichern und wiederherstellen kann, muss er vom Serialisierungs-Mixin erben und seine Attribute deklarieren.

1. Importieren Sie das Mixin in Ihre Datei:
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. Ändern Sie die Vererbung der Klasse, um es vor `QGraphicsObject` einzubeziehen:
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
    ```
3. Definieren Sie die Liste `SERIALISABLE` auf Klassenebene mit den Namen der Attribute, die Sie persistieren möchten. Nur einfache Typen (`int`, `float`, `str`, `bool`), Listen, Wörterbücher oder NumPy-Arrays werden unterstützt (letztere werden automatisch als `.npy`-Dateien innerhalb von `.sflow` gespeichert).
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frecuencia', 'amplitud', 'configuracion']
    ```
4. Stellen Sie sicher, dass diese Attribute in `__init__` initialisiert werden:
    ```python
    self.frecuencia = 1000.0
    self.amplitud = 1.0
    self.configuracion = {'tipo': 'seno', 'fase': 0}
    ```

Damit müssen Sie keine `serialize`/`deserialize`-Methoden schreiben; das Mixin kümmert sich automatisch um das Speichern und Wiederherstellen der Werte.

Wenn Ihr Knoten beim Laden zusätzliche Logik erfordert (z. B. ein Hardware-Gerät neu verbinden), können Sie `deserialize` überschreiben, nachdem Sie zuerst die Elternmethode aufgerufen haben:
```python
def deserialize(self, data):
    super().deserialize(data)   # stellt SERIALISABLE-Attribute wieder her
    self._iniciar_dispositivo()
```

---

## 4. Integration in das System

Sobald die Knotendatei erstellt ist, müssen Sie sie einfach in den Ordner `nodes` einfügen, damit sie in der Benutzeroberfläche erscheint und mit dem Rest des Systems funktioniert.

---

## 5. Internationalisierung (i18n)

Alle sichtbaren Texte sollten über `tr("schlüssel", default="...")` übersetzbar sein. Die Vorlage implementiert dies bereits. Sie müssen die entsprechenden Schlüssel in den JSON-Dateien innerhalb von `locales/` hinzufügen.

**Empfohlene Struktur:**
```json
{
   "nodes": {
     "mi_nodo": {
       "title": "Mi Nodo",
       "tooltip": "Descripción emergente",
       "ports": {
         "input": "Entrada",
         "output1": "Salida 1",
         "output2": "Salida 2"
      },
       "status": {
         "no_data": "Sin datos",
         "ready": "Listo"
      },
       "menu": {
         "show_output": "Mostrar salida",
         "configure": "Configurar..."
      },
       "help_title": "Ayuda - Mi Nodo",
       "help_html": "<h3>🎛️ Filter Node</h3>\n<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_mi_nodo": "Mi Nodo"
  }
}
```

Die HTML-Hilfe folgt dem für alle Knoten gemeinsamen Drei-Abschnitt-Format (spezifische Beschreibung + "So denkt man im System" + "Tastenkürzel und Tricks"). Die Vorlage enthält die Struktur bereits in `get_help_text()`.

---

## 6. Visuelle Themen

Die Methode `update_theme(self, theme)` erhält ein Wörterbuch mit den vom aktuellen Thema definierten Farben. Die Vorlage aktualisiert automatisch:
- Knotenhintergrund (`node_normal_bg`)
- Rahmen (`node_selected_border`)
- Titel- und Textfarbe (`node_normal_text`)
- Portfarben (`port_circle`, `port_outline`, `port_inline`, `port_text`)

Stellen Sie sicher, dass in `MainWindow` (oder `ThemeUpdater`) beim Themenwechsel `node.update_theme()` für jeden Knoten aufgerufen wird.

---

## 7. Checkliste und Fehlerbehebung

### ✅ Checkliste
- [ ] Der Knoten wird korrekt aus der Symbolleiste erstellt.
- [ ] Die Ports werden an den erwarteten Positionen angezeigt und sind für Verbindungen erkennbar (`Ctrl+Klick`).
- [ ] Beim Empfang von Eingabedaten wird `execute_program` aufgerufen und das Signal verarbeitet.
- [ ] Die Ausgaben werden korrekt an verbundene Knoten weitergegeben.
- [ ] Das Kontextmenü erlaubt das Ändern des Visualisierungskanals (falls mehrere Ausgänge vorhanden sind).
- [ ] Beim Klicken auf den Knoten wird das ausgewählte Signal im Plot-Widget dargestellt.
- [ ] Doppelklick öffnet die Hilfe im passenden Format.
- [ ] Die Sprache ändert sich korrekt (Titel-, Port- und Menütexte).
- [ ] Das Thema ändert sich korrekt (Farben des Knotens und der Ports).
- [ ] Kopieren/Einfügen funktioniert fehlerfrei.

!!! tip "Präzise Portverbindung"
    Stellen Sie beim Verbinden von Knoten sicher, dass Sie exakt auf den Kreis des Zielports klicken. Wenn Sie auf den Knotenkörper klicken, verwendet das System einen generischen Namen (`'data_in'`). Die Vorlage toleriert diese Namen jetzt, aber es ist eine gute Praxis, direkt auf den Kreis zu klicken, um die korrekte Routung mehrerer Ausgänge zu gewährleisten.

### 🐛 Häufige Probleme

| Symptom | Mögliche Ursache | Lösung |
|---------|------------------|--------|
| Der Verbindungspfeil verankert sich nicht am Port. | Der Portkreis hat kein `setData(0, port_name)` oder `get_port_scene_pos` ist nicht implementiert. | Überprüfen Sie, dass in `_create_ports` `circle.setData(0, port_name)` ausgeführt wird und `get_port_scene_pos` diesen Namen verwendet. |
| Die Ausgaben erreichen die verbundenen Knoten nicht. | `execute_program` gibt kein Wörterbuch zurück (für mehrere Ausgänge) oder `get_output_for_port` ist nicht implementiert. | Stellen Sie sicher, dass `execute_program` `{port_name: daten}` zurückgibt und `get_output_for_port` den entsprechenden Wert zurückgibt. |
| Beim Klicken auf den Knoten wird nichts im Plot dargestellt. | `get_display_signal` gibt kein gültiges Tupel `(x, y)` zurück oder `display_channel` stimmt nicht mit einer vorhandenen Ausgabe überein. | Überprüfen Sie, dass `get_display_signal` den ausgewählten Kanal verwendet und die Daten NumPy-Arrays sind. |
| Texte werden beim Sprachwechsel nicht aktualisiert. | Das Signal `languageChanged` wurde nicht verbunden oder `update_language` aktualisiert die Elemente nicht. | Überprüfen Sie die Verbindung in `__init__`: `language_manager.languageChanged.connect(self.update_language)`. |
| Das Thema wird nicht angewendet. | `update_theme` wird beim Erstellen des Knotens oder beim Themenwechsel nicht aufgerufen. | Rufen Sie in `MainWindow` nach der Knotenerstellung `node.update_theme(self.theme_manager.current_theme())` auf. |
| Der Pfeil zeigt auf die Knotenmitte. | Es wurde auf den Körper statt auf den Kreis geklickt, oder der Name stimmt nicht mit `PORTS` überein. | Klicken Sie direkt auf den Kreis. Überprüfen Sie, dass `get_port_scene_pos` die Alias-Zuordnung enthält. |
| `NameError: name 'self' is not defined` beim Importieren. | Instanzattribute wurden außerhalb von `__init__` deklariert. | Alle Attribute wie `self.mi_parametro` müssen innerhalb von `__init__` definiert werden. |
| Parameter gehen beim Kopieren/Öffnen von `.sflow` verloren. | Der Knoten erbt nicht von `SerializableMixin` oder `SERIALISABLE` ist nicht definiert. | Implementieren Sie Schritt 3.6 dieses Leitfadens. |

---

## 8. Fazit

Wenn Sie diesem Leitfaden folgen und die Vorlage `template_node.py` verwenden, können Sie FloWorks effizient und konsistent mit dem Rest des Systems neue Knoten hinzufügen. Denken Sie immer daran, die Kompatibilität mit i18n und Themen für ein professionelles Benutzererlebnis zu wahren.

Tragen Sie gerne mit Ihren eigenen Knoten bei!
