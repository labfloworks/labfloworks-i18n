---
title: Internationalisatie-gids (i18n)
description: Stapsgewijze instructies voor het toevoegen en beheren van vertalingen in FloWorks
---

# 🌐 Internationalisatie-gids (i18n)

Dit document legt uit hoe u een nieuwe taal toevoegt aan FloWorks en de vertaalbestanden effectief beheert.

---

## ➕ Een Nieuwe Taal Toevoegen

### Stap 1: Het JSON-bestand Aanmaken
Navigeer naar de map `locales/`. Kopieer `en.json` en hernoem het met de corresponderende tweeletterige [ISO 639-1](https://nl.wikipedia.org/wiki/ISO_639-1)-code (bijv. `fr.json` voor Frans, `de.json` voor Duits).

### Stap 2: De Tekenreeksen Vertalen
Open het nieuwe JSON-bestand in een teksteditor.

!!! warning "Wijzig de Sleutels Niet"
    **Wijzig nooit de sleutels** (de linkerkant van elk paar). Vertaal alleen de waarden (de rechterkant).

**Origineel (`en.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "File",
    "edit": "Edit"
  }
}
```

**Vertaald Voorbeeld (`es.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "Archivo",
    "edit": "Editar"
  }
}
```
Zorg ervoor dat de hoofdsleutel `"language_name"` de autochtone naam van de taal bevat (bijv. `"Français"`, `"Deutsch"`, `"Español"`).

### Stap 3: JSON Valideren
Controleer of het bestand geldige JSON is (geen sluitkomma's, correcte aanhalingstekens, juiste escapes). Je kunt online validators gebruiken zoals [JSONLint](https://jsonlint.com) of het volgende uitvoeren:
```bash
python -m json.tool locales/es.json
```

### Stap 4: De Nieuwe Taal Testen
1. Start FloWorks.
2. Ga naar **INFO → Taal** en selecteer de nieuwe taal.
3. Controleer of alle interface-elementen onmiddellijk worden bijgewerkt (menu's, panelen, dialoogvensters, knooppuntlabels, etc.).

### Stap 5: Automatische Detectie (Optioneel)
Als de systeemlocale van de gebruiker overeenkomt met de code van de nieuwe taal, zal FloWorks deze automatisch gebruiken bij de eerste start (mits er geen eerdere voorkeur is opgeslagen in `QSettings`).

---

## 🌍 Beschikbare Talen
- **Engels** (`en`) – Basis- / terugvaltaal
- **Spaans** (`es`)

---

## ⚙️ Belangrijke Opmerkingen en Beste Praktijken

!!! info "Terugvalmechanisme"
    De basistaal is **Engels**. Als een vertaalsleutel ontbreekt in een taalbestand, gebruikt FloWorks automatisch de Engelse tekenreeks als terugval.

!!! warning "Voorkomen van UI-overloop"
    Houd vertalingen beknopt om ontwerpbreuken te voorkomen. Als een vertaalde tekst aanzienlijk langer is, overweeg dan te verkorten of vertrouw op het themasysteem voor dynamische schaling.

!!! tip "HTML en Placeholders Behouden"
    - **HTML-tags:** Bewaar alle HTML-tags exact zoals ze zijn (bijv. `<h3>`, `<b>`, `<pre>`, `<br>`).
    - **Placeholders:** Behoud de syntaxis `{variable}` waar deze wordt gebruikt (bijv. `"Idioma cambiado a: {name} ({code})"`). Herschik of verwijder ze niet.

---

## 🔗 Gerelateerde Documentatie
- [📖 Codekaart en Architectuur](architecture-ii.md)
- [📦 Build- en Distributiegids](build.md)
- [🧩 Knooppuntreferentie](node-reference.md)
