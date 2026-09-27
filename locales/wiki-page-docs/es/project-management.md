---
title: Gestión de proyectos
description: Cómo guardar, abrir, exportar y proteger tus flujos en FloWorks.
---

# 📁 Gestión de proyectos

FloWorks guarda tus flujos en archivos con extensión **`.sflow`**. Estos archivos contienen toda la información del proyecto: nodos, conexiones, configuración y notas adhesivas.

---

## Crear, abrir y guardar

| Acción | Menú | Atajo |
|--------|------|-------|
| **Nuevo proyecto** | Archivo → Nuevo | `Ctrl + N` |
| **Abrir proyecto** | Archivo → Abrir | `Ctrl + O` |
| **Guardar** | Archivo → Guardar | `Ctrl + S` |
| **Guardar como…** | Archivo → Guardar como… | `Ctrl + Shift + S` |

**Regla de Oro:**  
Los flujos son **completamente compatibles entre todas las versiones** de FloWorks (Core, Lite, Pro). No necesitas convertir ni modificar nada: solo abrir y ejecutar.

---

## Exportación e importación

- Para **compartir un flujo**, copia el archivo `.sflow` a otro equipo.
- Para **traer un flujo externo**, usa **Archivo → Abrir** y selecciona el archivo.
- Si necesitas **exportar datos numéricos** (por ejemplo, a CSV), utiliza la herramienta **Hoja de Cálculo** del panel lateral y guarda la tabla desde allí.

---

## Recuperación ante cierres inesperados

FloWorks **no guarda automáticamente**. Por eso es importante:

- Guardar con frecuencia (`Ctrl + S`), especialmente antes de ejecutar flujos con hardware real.
- Si la aplicación se cierra de forma inesperada, los cambios no guardados podrían perderse.
- Para trabajar con total tranquilidad, acostúmbrate a guardar después de cada modificación importante.

---

## Organización recomendada

- Crea una carpeta por cada proyecto o cliente, y guarda allí todos los `.sflow` relacionados.
- Utiliza **notas adhesivas** dentro del lienzo para documentar secciones del flujo.
- Asigna **nombres descriptivos a los nodos** (doble clic → nombre) para que sea más fácil encontrar y entender el flujo semanas después.

---

## Buenas prácticas

- Antes de ejecutar un flujo con instrumentos reales, guarda el archivo.
- Si trabajas en equipo, usa un sistema de control de versiones (Git, copias manuales) para no sobrescribir flujos importantes.
- Haz copias de seguridad de flujos de calibración o diagnóstico críticos.

---

> **Consejo:** Un flujo bien organizado y guardado es la base de un trabajo profesional en FloWorks. No subestimes el poder de un nombre claro y una carpeta ordenada.