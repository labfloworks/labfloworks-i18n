---
title: Projektverwaltung
description: Wie man Flüsse in FloWorks speichert, öffnet, exportiert und schützt.
---

# 📁 Projektverwaltung

FloWorks speichert Ihre Flüsse in Dateien mit der Erweiterung **`.sflow`**. Diese Dateien enthalten alle Projektinformationen: Knoten, Verbindungen, Konfiguration und Haftnotizen.

---

## Erstellen, Öffnen und Speichern

| Aktion | Menü | Tastenkürzel |
|--------|------|-------|
| **Neues Projekt** | Datei → Neu | `Ctrl + N` |
| **Projekt öffnen** | Datei → Öffnen | `Ctrl + O` |
| **Speichern** | Datei → Speichern | `Ctrl + S` |
| **Speichern unter…** | Datei → Speichern unter… | `Ctrl + Shift + S` |

**Goldene Regel:**  
Die Flüsse sind **vollständig kompatibel zwischen allen Versionen** von FloWorks (Core, Lite, Pro). Sie müssen nichts konvertieren oder ändern: einfach öffnen und ausführen.

---

## Export und Import

- Um **einen Fluss zu teilen**, kopieren Sie die `.sflow`-Datei auf einen anderen Computer.
- Um **einen externen Fluss zu importieren**, verwenden Sie **Datei → Öffnen** und wählen Sie die Datei aus.
- Wenn Sie **numerische Daten exportieren** müssen (z. B. nach CSV), verwenden Sie das Tool **Tabellenkalkulation** im Seitenpanel und speichern Sie die Tabelle von dort aus.

---

## Wiederherstellung bei unerwartetem Schließen

FloWorks **speichert nicht automatisch**. Daher ist es wichtig:

- Häufig zu speichern (`Ctrl + S`), besonders bevor Flüsse mit echter Hardware ausgeführt werden.
- Wenn die Anwendung unerwartet geschlossen wird, können nicht gespeicherte Änderungen verloren gehen.
- Um völlig beruhigt zu arbeiten, gewöhnen Sie sich an, nach jeder wichtigen Änderung zu speichern.

---

## Empfohlene Organisation

- Erstellen Sie einen Ordner für jedes Projekt oder jeden Kunden und speichern Sie dort alle zugehörigen `.sflow`-Dateien.
- Verwenden Sie **Haftnotizen** innerhalb der Zeichenfläche, um Abschnitte des Flusses zu dokumentieren.
- Weisen Sie den **Knoten beschreibende Namen** zu (Doppelklick → Name), damit es später leichter ist, den Fluss zu finden und zu verstehen.

---

## Gute Praktiken

- Speichern Sie die Datei, bevor Sie einen Fluss mit echten Geräten ausführen.
- Wenn Sie im Team arbeiten, verwenden Sie ein Versionskontrollsystem (Git, manuelle Kopien), um wichtige Flüsse nicht zu überschreiben.
- Erstellen Sie Sicherungskopien kritischer Kalibrierungs- oder Diagnoseflüsse.

---

> **Tipp:** Ein gut organisierter und gespeicherter Fluss ist die Grundlage für professionelle Arbeit in FloWorks. Unterschätzen Sie nicht die Kraft eines klaren Namens und eines geordneten Ordners.
