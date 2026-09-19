# FloWorks Internationalization (i18n)

Help us translate FloWorks into your native language.

## About Us

We are a small team based in Colombia. We have very limited resources for a project of this size, but we want FloWorks to reach users around the world. We know that translation keys and translated texts may contain errors, so we welcome collaboration to find and correct them. Every contribution helps us improve.

We maintain a strict key-value balance across all languages to ensure the user interface loads seamlessly.

## How to Contribute

### Adding a New Language

1. Fork this repository.
2. Go to the `locales/` directory.
3. Duplicate `en.json` (or `es.json`).
4. Rename the file using the 2-letter ISO code for your language (e.g., `ca.json` for Catalan).
5. Translate the values on the right.

**Strict requirement:** The new file must have exactly the same structure and keys as `en.json` / `es.json`. Every key present in the base files must exist in your new file. The important part is that the JSON structure and keys match, not the raw line count.

### Updating an Existing Language

1. Open the `.json` file for your language inside `locales/`.
2. Update or refine any inaccurate translation values.
3. Keep the file synchronized with the base files in structure and keys.

## Translation Rules

- Do not modify the JSON keys (left side). Only translate the string values (right side).
  - Incorrect: `"btn_cancel": "Cancelar"` — do not edit `"btn_cancel"`.
  - Correct: `"btn_cancel": "Cancel·lar"`.
- Keep placeholders intact. If a value contains variable tokens such as `{count}`, `{filename}`, or `%s`, leave them exactly as they are.
- Valid JSON format. Ensure there are no trailing commas on the last lines.
- Use UTF-8 encoding.

## Submitting Your Work

Once you finish, open a Pull Request (PR).

Title format:
- `i18n: add Catalan translation (ca.json)`
- `i18n: update French locale`

Our team will review the JSON syntax and merge it into the official release.

Thank you for helping make FloWorks accessible worldwide.
Nuestro equipo revisará la sintaxis del JSON y la integrará en la versión oficial.

Thank you for helping make FloWorks accessible worldwide.  
Gracias por ayudarnos a que FloWorks sea accesible en todo el mundo.
