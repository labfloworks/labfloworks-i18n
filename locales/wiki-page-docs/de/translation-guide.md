---
title: Internationalisierungsleitfaden (i18n)
description: Schritt-für-Schritt-Anleitung zum Hinzufügen und Verwalten von Übersetzungen in FloWorks
---

# 🌐 Internationalisierungsleitfaden (i18n)

Dieses Dokument erklärt, wie man FloWorks eine neue Sprache hinzufügt und die Übersetzungsdateien effektiv verwaltet.

---

## ➕ So fügen Sie eine neue Sprache hinzu

### Schritt 1: Die JSON-Datei erstellen
Navigieren Sie zum Ordner `locales/`. Kopieren Sie `en.json` und benennen Sie es mit dem entsprechenden zweibuchstabigen [ISO 639-1](https://es.wikipedia.org/wiki/ISO_639-1)-Code um (z. B. `fr.json` für Französisch, `de.json` für Deutsch).

### Schritt 2: Die Zeichenketten übersetzen
Öffnen Sie die neue JSON-Datei in einem Texteditor.

!!! warning "Die Schlüssel nicht ändern"
    **Ändern Sie niemals die Schlüssel** (die linke Seite jedes Paares). Übersetzen Sie nur die Werte (die rechte Seite).

**Original (`en.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "File",
    "edit": "Edit"
  }
}
```

**Übersetztes Beispiel (`es.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "Archivo",
    "edit": "Editar"
  }
}
```
Stellen Sie sicher, dass der Stammschlüssel `"language_name"` den Muttersprachennamen enthält (z. B. `"Français"`, `"Deutsch"`, `"Español"`).

### Schritt 3: Das JSON validieren
Überprüfen Sie, ob die Datei gültiges JSON ist (keine abschließenden Kommas, korrekte Anführungszeichen, angemessene Escapes). Sie können Online-Validatoren wie [JSONLint](https://jsonlint.com) verwenden oder ausführen:
```bash
python -m json.tool locales/es.json
```

### Schritt 4: Die neue Sprache testen
1. Starten Sie FloWorks.
2. Gehen Sie zu **INFO → Sprache** und wählen Sie die neue Sprache aus.
3. Überprüfen Sie, ob alle Elemente der Benutzeroberfläche sofort aktualisiert werden (Menüs, Paneele, Dialoge, Knotenbeschriftungen usw.).

### Schritt 5: Automatische Erkennung (optional)
Wenn das Gebietsschema des Benutzersystems mit dem Code der neuen Sprache übereinstimmt, verwendet FloWorks sie automatisch beim ersten Start (sofern keine vorherige Präferenz in `QSettings` gespeichert wurde).

---

## 🌍 Verfügbare Sprachen
- **Englisch** (`en`) – Basis- / Fallback-Sprache
- **Spanisch** (`es`)

---

## ⚙️ Wichtige Hinweise und Best Practices

!!! info "Fallback-Mechanismus"
    Die Basissprache ist **Englisch**. Wenn in einer Sprachdatei ein Übersetzungsschlüssel fehlt, verwendet FloWorks automatisch die englische Zeichenkette als Fallback.

!!! warning "Verhinderung von UI-Überlauf"
    Halten Sie Übersetzungen prägnant, um Layoutbrüche zu vermeiden. Wenn ein übersetzter Text deutlich länger ist, erwägen Sie eine Abkürzung oder verlassen Sie sich auf das Designsystem, um die dynamische Skalierung zu handhaben.

!!! tip "HTML und Platzhalter beibehalten"
    - **HTML-Tags:** Bewahren Sie alle HTML-Tags genau so bei, wie sie sind (z. B. `<h3>`, `<b>`, `<pre>`, `<br>`).
    - **Platzhalter:** Behalten Sie die Syntax `{variable}` dort bei, wo sie verwendet wird (z. B. `"Sprache geändert zu: {name} ({code})"`). Reordenieren oder entfernen Sie sie nicht.

---

## 🔗 Verwandte Dokumentation
- [📖 Code-Karte und Architektur](architecture-ii.md)
- [📦 Build- und Vertriebsleitfaden](build.md)
- [🧩 Knotenreferenz](node-reference.md)
