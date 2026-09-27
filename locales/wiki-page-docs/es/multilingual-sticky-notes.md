# Sticky Notes (Notas adhesivas) – Guía del usuario

## ¿Qué son las notas adhesivas?

Las notas adhesivas (o *sticky notes*) son pequeños bloques de texto que puede colocar libremente sobre el diagrama. Sirven para:

- Añadir recordatorios, títulos o explicaciones directamente en el lienzo.
- Crear tutoriales paso a paso que guíen a quien utilice su proyecto.
- Documentar partes del flujo de trabajo sin necesidad de salir de FloWorks.
- Dejar comentarios para usted mismo o para otros colaboradores.

Las notas son redimensionables (arrastrando sus esquinas), se mueven a cualquier lugar del diagrama y se guardan junto con el proyecto. Al abrir un archivo `.sflow`, todas las notas aparecen exactamente donde las dejó.

---

## La novedad: notas multidioma

Las notas adhesivas pueden mostrar automáticamente el texto en el idioma que usted elija para la aplicación.  
En lugar de escribir el mensaje final en un solo idioma, puede insertar **marcadores especiales** que se traducirán solos al cambiar el idioma de FloWorks.

De este modo, una misma nota puede leerse en español, inglés o cualquier otro idioma disponible sin necesidad de editar el texto cada vez.

---

## Cómo escribir una nota multidioma

Dentro de una nota (cree una con doble clic o con el botón 📝 de la barra de herramientas), puede usar dos tipos de marcadores:

### 1. Con la palabra `tr(…)`
Escriba `tr("clave")` y reemplace `clave` por un nombre descriptivo de la frase.

Ejemplo:

tr("tutorial.paso1.titulo")
tr("tutorial.paso1.mensaje")

### 2. Con dobles llaves `{{…}}`
Escriba `{{clave}}` de la misma manera.

Ejemplo:

{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}


Ambos formatos funcionan igual; elija el que le resulte más cómodo (incluso puede combinarlos en la misma nota).

> **Importante**: El texto que usted ve cuando edita la nota contiene los marcadores originales (por ejemplo `{{tutorial.paso1.titulo}}`).  
> Al terminar de editar y volver a la vista normal del diagrama, los marcadores se reemplazan por la frase traducida al idioma actual de la aplicación.

---

## Comportamiento cuando cambia el idioma

- Si usted cambia el idioma desde el menú de FloWorks (por ejemplo, de español a inglés), **todas las notas adhesivas que contengan marcadores se actualizan automáticamente**.
- No es necesario cerrar y volver a abrir el proyecto, ni tocar cada nota manualmente.
- Las notas que sólo contienen texto normal (sin marcadores) no se ven afectadas; muestran lo mismo en cualquier idioma.

---

## Ventajas de usar marcadores

- **Tutoriales multidioma instantáneos** – Una misma nota sirve para guiar a usuarios de distintos idiomas.
- **Coherencia** – Si modifica la traducción en un solo lugar (el archivo de idiomas que su equipo de desarrollo mantiene), todas las notas que usen esa clave se actualizarán.
- **Mantenimiento sencillo** – Puede escribir el contenido una sola vez y reutilizarlo en múltiples notas.
- **Flexibilidad** – Combine texto fijo con marcadores. Por ejemplo:

---

## Ejemplo práctico: un tutorial paso a paso

Suponga que quiere añadir una nota que explique el primer paso de un tutorial.  
En modo edición escribe:

🎯 PASO 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}


Cuando termine de editar y esté usando la aplicación en español, verá:

🎯 PASO 1
¡Bienvenido a FloWorks!
Arrastre un nodo fuente de señal para comenzar.


Si cambia el idioma a inglés, la misma nota mostrará:


🎯 PASO 1
Welcome to FloWorks!
Drag a signal source node to begin.


Y así con cualquier otro idioma que tenga configurado.

---

## Resumen

- Las notas adhesivas enriquecen sus diagramas con información textual.
- Ahora pueden ser **multilingües** usando los marcadores `tr("clave")` o `{{clave}}`.
- Al editar verá las claves; al visualizar, el texto traducido.
- Cambie el idioma de la aplicación y todas las notas se adaptarán al instante.
- Perfecto para crear documentación visual, tutoriales o avisos que deban funcionar en varios idiomas.

¡Aproveche esta funcionalidad para hacer sus proyectos más accesibles y fáciles de compartir!