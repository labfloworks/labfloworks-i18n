---
title: Ghid de Internaționalizare (i18n)
description: Instrucțiuni pas cu pas pentru adăugarea și gestionarea traducerilor în FloWorks
---

# 🌐 Ghid de Internaționalizare (i18n)

Acest document explică cum să adăugați o nouă limbă în FloWorks și să gestionați eficient fișierele de traducere.

---

## ➕ Cum se Adaugă o Limbă Nouă

### Pasul 1: Crearea Fișierului JSON
Navigați la folderul `locales/`. Copiați `en.json` și redenumiți-l folosind codul din două litere [ISO 639-1](https://ro.wikipedia.org/wiki/ISO_639-1) corespunzător (ex. `fr.json` pentru franceză, `de.json` pentru germană).

### Pasul 2: Traducerea Șirurilor
Deschideți noul fișier JSON într-un editor de text.

!!! warning "Nu Modificați Cheile"
    **Nu schimbați niciodată cheile** (partea stângă a fiecărei perechi). Traduceți doar valorile (partea dreaptă).

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

**Exemplu Tradus (`es.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "Archivo",
    "edit": "Editar"
  }
}
```
Asigurați-vă că cheia rădăcină `"language_name"` conține numele nativ al limbii (ex. `"Français"`, `"Deutsch"`, `"Español"`).

### Pasul 3: Validarea JSON
Verificați că fișierul este JSON valid (fără virgule finale, ghilimele corecte, escape-uri adecvate). Puteți folosi validatoare online precum [JSONLint](https://jsonlint.com) sau rulați:
```bash
python -m json.tool locales/es.json
```

### Pasul 4: Testarea Noii Limbi
1. Porniți FloWorks.
2. Mergeți la **INFO → Limbă** și selectați noua limbă.
3. Verificați că toate elementele interfeței se actualizează imediat (meniuri, panouri, dialoguri, etichete noduri etc.).

### Pasul 5: Detectare Automată (Opțional)
Dacă setările regionale ale sistemului utilizatorului coincid cu codul noii limbi, FloWorks o va utiliza automat la prima pornire (atâta timp cât nu a fost salvată o preferință anterioară în `QSettings`).

---

## 🌍 Limbi Disponibile
- **Engleză** (`en`) – Limbă de bază / de rezervă
- **Spaniolă** (`es`)

---

## ⚙️ Note Importante și Bune Practici

!!! info "Mecanism de Rezervă"
    Limba de bază este **engleza**. Dacă lipsește o cheie de traducere într-un fișier de limbă, FloWorks folosește automat șirul în engleză ca rezervă.

!!! warning "Prevenirea Depășirii UI"
    Păstrați traducerile concise pentru a evita rupturile de design. Dacă un text tradus este semnificativ mai lung, considerați abrevierea sau încredințați sistemului de teme gestionarea scalării dinamice.

!!! tip "Păstrarea HTML și a Placeholderelor"
    - **Etichete HTML:** Păstrați toate etichetele HTML exact așa cum sunt (ex. `<h3>`, `<b>`, `<pre>`, `<br>`).
    - **Placeholdere:** Mențineți sintaxa `{variable}` unde este utilizată (ex. `"Idioma cambiado a: {name} ({code})"`). Nu le reordonați și nu le eliminați.

---

## 🔗 Documentație Conexă
- [📖 Harta Codului și Arhitectura](architecture-ii.md)
- [📦 Ghid de Build și Distribuire](build.md)
- [🧩 Referință Noduri](node-reference.md)
