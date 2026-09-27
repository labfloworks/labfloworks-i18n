---
title: Internationalization (i18n) Guide
description: Step-by-step instructions for adding and managing translations in FloWorks
---

# 🌐 Internationalization (i18n) Guide

This document explains how to add a new language to FloWorks and manage translation files effectively.

---

## ➕ How to Add a New Language

### Step 1: Create the JSON File
Navigate to the `locales/` folder. Copy `en.json` and rename it using the corresponding two-letter [ISO 639-1](https://en.wikipedia.org/wiki/ISO_639-1) code (e.g. `fr.json` for French, `de.json` for German).

### Step 2: Translate the Strings
Open the new JSON file in a text editor.

!!! warning "Do Not Modify the Keys"
    **Never change the keys** (the left side of each pair). Only translate the values (the right side).

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

**Translated Example (`es.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "Archivo",
    "edit": "Editar"
  }
}
```
Make sure the root key `"language_name"` contains the native name of the language (e.g. `"Français"`, `"Deutsch"`, `"Español"`).

### Step 3: Validate the JSON
Verify that the file is valid JSON (no trailing commas, correct quotes, proper escapes). You can use online validators such as [JSONLint](https://jsonlint.com) or run:
```bash
python -m json.tool locales/es.json
```

### Step 4: Test the New Language
1. Start FloWorks.
2. Go to **INFO → Language** and select the new language.
3. Verify that all interface elements update immediately (menus, panels, dialogs, node labels, etc.).

### Step 5: Automatic Detection (Optional)
If the user's system locale matches the new language code, FloWorks will use it automatically on first launch (as long as no previous preference has been saved in `QSettings`).

---

## 🌍 Available Languages
- **English** (`en`) – Base / fallback language
- **Spanish** (`es`)

---

## ⚙️ Important Notes and Good Practices

!!! info "Fallback Mechanism"
    The base language is **English**. If a translation key is missing in a language file, FloWorks automatically uses the English string as fallback.

!!! warning "UI Overflow Prevention"
    Keep translations concise to avoid layout breakage. If a translated text is significantly longer, consider abbreviating or rely on the theme system to handle dynamic scaling.

!!! tip "Preserve HTML and Placeholders"
    - **HTML tags:** Keep all HTML tags exactly as they are (e.g. `<h3>`, `<b>`, `<pre>`, `<br>`).
    - **Placeholders:** Maintain the `{variable}` syntax wherever it is used (e.g. `"Language changed to: {name} ({code})"`). Do not reorder or remove them.

---

## 🔗 Related Documentation
- [📖 Code Map and Architecture](architecture-ii.md)
- [📦 Build and Distribution Guide](build.md)
- [🧩 Node Reference](node-reference.md)
