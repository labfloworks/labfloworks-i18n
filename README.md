# FloWorks Internationalization (i18n)

Help us translate FloWorks into your native language.  
Ayúdanos a traducir FloWorks a tu idioma nativo.

## About Us / Sobre nosotros

We are a small team based in Colombia. We have very limited resources for a project of this size, but we want FloWorks to reach users around the world. We know that translation keys and translated texts may contain errors, so we welcome collaboration to find and correct them. Every contribution helps us improve.

Somos un pequeño grupo de trabajo en Colombia. Contamos con muy pocos recursos para un proyecto de esta magnitud, pero queremos que FloWorks tenga la mayor acogida posible en todo el mundo. Sabemos que pueden existir errores en las claves de traducción o en los textos traducidos, por lo que agradecemos la colaboración para encontrarlos y corregirlos. Cada contribución nos ayuda a mejorar.

We maintain a strict key-value balance across all languages to ensure the user interface loads seamlessly.  
Mantenemos un equilibrio estricto entre claves y valores en todos los idiomas para que la interfaz se cargue sin problemas.

## How to Contribute / Cómo contribuir

### Adding a New Language / Añadir un nuevo idioma

1. Fork this repository.
2. Go to the `locales/` directory.
3. Duplicate `en.json` (or `es.json`).
4. Rename the file using the 2-letter ISO code for your language (e.g., `ca.json` for Catalan).
5. Translate the values on the right.

1. Haz un fork de este repositorio.
2. Ve al directorio `locales/`.
3. Duplica `en.json` (o `es.json`).
4. Renombra el archivo usando el código ISO de dos letras para tu idioma (por ejemplo, `ca.json` para catalán).
5. Traduce los valores de la derecha.

**Strict requirement / Requisito estricto:**

The new file must have exactly the same structure and line count as `en.json` / `es.json`. Every key present in the base files must exist in your new file.

El nuevo archivo debe tener exactamente la misma estructura y cantidad de líneas que `en.json` / `es.json`. Cada clave presente en los archivos base debe existir en tu nuevo archivo.

### Updating an Existing Language / Actualizar un idioma existente

1. Open the `.json` file for your language inside `locales/`.
2. Update or refine any inaccurate translation values.
3. Keep the file synchronized with the line count of the base files.

1. Abre el archivo `.json` de tu idioma dentro de `locales/`.
2. Actualiza o mejora cualquier traducción inexacta.
3. Mantén el archivo sincronizado con la cantidad de líneas de los archivos base.

## Translation Rules / Reglas de traducción

- Do not modify the JSON keys (left side). Only translate the string values (right side).
- No modifiques las claves JSON (lado izquierdo). Traduce únicamente los valores de cadena (lado derecho).

Examples / Ejemplos:

- Incorrect / Incorrecto: `"btn_cancel": "Cancelar"` — do not edit `"btn_cancel"`.
- Correct / Correcto: `"btn_cancel": "Cancel·lar"`.

- Keep placeholders intact. If a value contains variable tokens such as `{count}`, `{filename}`, or `%s`, leave them exactly as they are.
- Mantén intactos los placeholders. Si un valor contiene variables como `{count}`, `{filename}` o `%s`, déjalas exactamente como están.

- Valid JSON format. Ensure there are no trailing commas on the last lines.
- Formato JSON válido. Asegúrate de no dejar comas finales en las últimas líneas.

## Submitting Your Work / Enviar la contribución

Once you finish, open a Pull Request (PR).  
Cuando termines, abre un Pull Request (PR).

Title format / Formato del título:

- `i18n: add Catalan translation (ca.json)`
- `i18n: update French locale`

Our team will review the JSON syntax and merge it into the official release.  
Nuestro equipo revisará la sintaxis del JSON y la integrará en la versión oficial.

Thank you for helping make FloWorks accessible worldwide.  
Gracias por ayudarnos a que FloWorks sea accesible en todo el mundo.
