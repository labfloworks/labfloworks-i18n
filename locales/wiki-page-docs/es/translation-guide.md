---
title: Guía de Internacionalización (i18n)
description: Instrucciones paso a paso para añadir y gestionar traducciones en FloWorks
---

# 🌐 Guía de Internacionalización (i18n)

Este documento explica cómo añadir un nuevo idioma a FloWorks y gestionar eficazmente los archivos de traducción.

---

## ➕ Cómo Añadir un Nuevo Idioma

### Paso 1: Crear el Archivo JSON
Navega a la carpeta `locales/`. Copia `en.json` y renómbralo usando el código [ISO 639-1](https://es.wikipedia.org/wiki/ISO_639-1) de dos letras correspondiente (ej. `fr.json` para francés, `de.json` para alemán).

### Paso 2: Traducir las Cadenas
Abre el nuevo archivo JSON en un editor de texto.

!!! warning "No Modificar las Claves"
    **Nunca cambies las claves** (el lado izquierdo de cada par). Solo traduce los valores (el lado derecho).

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

**Ejemplo Traducido (`es.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "Archivo",
    "edit": "Editar"
  }
}
```
Asegúrate de que la clave raíz `"language_name"` contenga el nombre nativo del idioma (ej. `"Français"`, `"Deutsch"`, `"Español"`).

### Paso 3: Validar el JSON
Verifica que el archivo sea JSON válido (sin comas finales, comillas correctas, escapes apropiados). Puedes usar validadores en línea como [JSONLint](https://jsonlint.com) o ejecutar:
```bash
python -m json.tool locales/es.json
```

### Paso 4: Probar el Nuevo Idioma
1. Inicia FloWorks.
2. Ve a **INFO → Idioma** y selecciona el nuevo idioma.
3. Verifica que todos los elementos de la interfaz se actualicen inmediatamente (menús, paneles, diálogos, etiquetas de nodos, etc.).

### Paso 5: Detección Automática (Opcional)
Si la configuración regional del sistema del usuario coincide con el código del nuevo idioma, FloWorks lo usará automáticamente en el primer inicio (siempre que no se haya guardado una preferencia previa en `QSettings`).

---

## 🌍 Idiomas Disponibles
- **Inglés** (`en`) – Idioma base / de respaldo
- **Español** (`es`)

---

## ⚙️ Notas Importantes y Buenas Prácticas

!!! info "Mecanismo de Respaldo"
    El idioma base es **inglés**. Si falta una clave de traducción en un archivo de idioma, FloWorks usa automáticamente la cadena en inglés como fallback.

!!! warning "Prevención de Desbordamiento de UI"
    Mantén las traducciones concisas para evitar rupturas de diseño. Si un texto traducido es significativamente más largo, considera abreviar o confiar en el sistema de temas para manejar el escalado dinámico.

!!! tip "Preservar HTML y Placeholders"
    - **Etiquetas HTML:** Conserva todas las etiquetas HTML exactamente como están (ej. `<h3>`, `<b>`, `<pre>`, `<br>`).
    - **Placeholders:** Mantén la sintaxis `{variable}` donde se use (ej. `"Idioma cambiado a: {name} ({code})"`). No los reordenes ni elimines.

---

## 🔗 Documentación Relacionada
- [📖 Mapa de Código y Arquitectura](architecture-ii.md)
- [📦 Guía de Build y Distribución](build.md)
- [🧩 Referencia de Nodos](node-reference.md)