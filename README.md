# 🌍 FloWorks Internationalization (i18n)

Help us translate **FloWorks** into your native language!

We maintain a strict key-value balance across all languages to ensure the user interface loads seamlessly.

---

## 🚀 How to Contribute / Cómo Contribuir

### Adding a New Language / Añadir un nuevo idioma
1. **Fork** this repository.
2. Go to the `locales/` directory.
3. Duplicate `en.json` (or `es.json`).
4. Rename the file using the 2-letter ISO code for your language (e.g., `ca.json` for Catalan, `de.json` for German, `fr.json` for French).
5. Translate the values on the right. 

> ⚠️ **STRICT REQUIREMENT:**  
> The new file **MUST HAVE EXACTLY THE SAME STRUCTURE AND LINE COUNT** as `en.json` / `es.json`. Every key present in the base files must exist in your new file.

---

### Updating an Existing Language / Actualizar un idioma existente
1. Open the `.json` file for your language inside `locales/`.
2. Update or refine any inaccurate translation values.
3. Keep the file synchronized with the line count of the base files.

---

## ⚠️ Translation Rules / Reglas de Traducción

* **DO NOT modify the JSON keys (left side).** Only translate the string values (right side).
  * ❌ `"btn_cancel": "Cancelar"` $\rightarrow$ Do not edit `"btn_cancel"`.
  * ✅ `"btn_cancel": "Cancel·lar"`
* **Keep placeholders intact:** If a value contains variable tokens like `{count}`, `{filename}`, or `%s`, leave them exactly as they are.
* **Valid JSON format:** Ensure there are no trailing commas on the last lines.

---

## 📥 Submitting Your Work / Enviar la contribución

Once you finish, open a **Pull Request (PR)**:
* **Title format:** `i18n: add Catalan translation (ca.json)` or `i18n: update French locale`
* Our team will review the JSON syntax and merge it into the official release.

Thank you for helping make **FloWorks** accessible worldwide! 🚀
