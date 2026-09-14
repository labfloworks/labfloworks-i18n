# 🌍 FloWorks Internationalization (i18n)

Help us translate **FloWorks** into your native language!

## 🚀 How to contribute

1. **Fork** this repository.
2. Go to the `locales/` directory.
3. To update an existing language:
   - Edit the corresponding `.json` file (e.g., `fr.json`).
4. To add a new language:
   - Copy `locales/en.json` (or `locales/template.json`).
   - Rename it using the standard ISO language code (e.g., `de.json` for German, `ja.json` for Japanese).
   - Translate the values (keep the **keys** intact).
5. Submit a **Pull Request (PR)** with a clear title (e.g., `feat: add German translation`).

## ⚠️ Important Rules for Translators

- **DO NOT** change the JSON keys (left side). Only translate the values (right side).
  - ❌ `"btn_cancel": "Cancelar"` → Do not change `"btn_cancel"`.
  - ✅ `"btn_cancel": "Abbrechen"`
- Keep string placeholders intact (e.g., `{count}`, `%s`, `{name}`).
- Ensure the JSON file remains valid format (no trailing commas).
