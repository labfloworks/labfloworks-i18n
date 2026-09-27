---
title: Primeros pasos con FloWorks
description: Guía rápida para configurar el entorno, ejecutar tu primer flujo y acceder a la versión portable.
---

# 🚀 Primeros pasos con FloWorks

Esta guía te llevará desde cero hasta tener tu primer flujo de procesamiento de señales ejecutándose. FloWorks es una aplicación de diagramas de flujo para señales, construida con Python y PySide6, que soporta hardware real (VISA/SCPI), simulación integrada, scripting avanzado y cambio de idioma en caliente.

---

## 🌊 Tu primer flujo de ejemplo

Vamos a crear un flujo sencillo: generar una señal senoidal y visualizarla en tiempo real.

1. **Añadir nodos**
   En la barra de herramientas superior, selecciona `Fuente` → selecciona `Generador de señales Avanzado`. Luego, desde `Procesamiento` → selecciona, por ejemplo, `Espectral`.
2. **Conectar**
   Haz `Ctrl+Clic` en el puerto de salida (`right`) del generador. Luego, haz `Clic` en el puerto de entrada (`left`) del osciloscopio. O simplemente dar clic en el pueto de salida y arrastar (manteniendo el clic presionado) hasta el puerto de entrada del siguiente nodo.
3. **Configurar (opcional)**
   Clic en un nodo, en el panel lateral izquierdo aparecerá un editor con los parámetros del nodo que tienes seleccionado para ajustar las condiciones de operación. En la parte inferior hay una grafica de visualización que representa visualmente los datos generados o adquiridos por los nodos.
4. **Ejecutar**
   Presiona `F5` o el botón ▶ en la toolbar. El motor topológico calculará el orden de ejecución, procesará los datos y verás la onda en el panel gráfico. ¡El conector se animará indicando el flujo activo!


---

## 🧠 Entendiendo los puertos: categorías por color

En FloWorks, cada puerto pertenece a una **categoría funcional** identificada por un color. Las conexiones válidas se hacen **siempre entre puertos del mismo color**: una salida de una categoría se conecta únicamente con una entrada de la misma categoría. Además, la línea conectora adopta automáticamente el color de los puertos que une, facilitando la lectura visual.

| Tipo | Color | Propósito | Ejemplo típico |
|------|-------|-----------|----------------|
| `control` | Blanco  | Flujo de control / activación. | Señal de arranque hacia un nodo de adquisición. |
| `exec` | Gris | Ejecución de operaciones o pasos. | Disparo de una función o callback. |
| `data` | Verde  | Datos genéricos / señales numéricas. | Salida de un generador o sensor. |
| `int` | Azul  | Números enteros. | Índice, tamaño de buffer, ID. |
| `float` | Cian  | Números de punto flotante. | Amplitud, frecuencia, umbral. |
| `string` | Púrpura  | Cadenas de texto. | Nombre de archivo, etiqueta. |
| `bool` | Rosa  | Valores booleanos (`True`/`False`). | Bandera de estado, habilitación. |
| `array` | Azul oscuro  | Arreglos / vectores. | Señal multicanal, lista de muestras. |
| `trigger` | Naranja  | Disparos / eventos discretos. | Pulso de sincronización, flanco. |

**Regla de oro:**

- Solo se conectan puertos del **mismo color exacto** (salida ↔ entrada de la misma categoría).
- El sistema evita conexiones inválidas y resalta visualmente los puertos compatibles al arrastrar.
- La línea conectora toma el color de los puertos conectados; así cada ruta se identifica de un vistazo.

**Filosofía FloWorks:**  
Los puertos de datos **preservan la dimensionalidad** de los arrays. Nunca se aplica un aplanado automático: si entra una matriz, sale una matriz, manteniendo la integridad de tus señales multidimensionales.

![FloWorks](assets/tipos_de_puertos.PNG)

---

## 🖱️ Navegación por el Lienzo (Canvas)

Domina el espacio de trabajo con estos gestos:

| Acción | Cómo hacerlo |
|--------|--------------|
| **Zoom** | Rueda del ratón o `Ctrl + rueda` |
| **Paneo (desplazarse)** | Mantén `Espacio` y arrastra, o usa el botón central del ratón |
| **Seleccionar un nodo** | Clic izquierdo sobre el nodo |
| **Selección múltiple** | Arrastra un rectángulo con clic izquierdo, o `Ctrl + clic` en varios nodos |
| **Mover selección** | Arrastra cualquiera de los nodos seleccionados |
| **Abrir configuración** | Doble clic sobre un nodo |

**Consejo:** El panel izquierdo se actualiza automáticamente con la configuración del nodo seleccionado, sin necesidad de abrir ventanas adicionales.

---

## ⚡ Atajos de teclado y movimientos avanzados

Estos atajos convierten a un usuario normal en un **power user**:

| Atajo | Acción |
|-------|--------|
| `F5` | Ejecutar flujo |
| `Ctrl + S` | Guardar proyecto (`.sflow`) |
| `Ctrl + Clic` | Conectar nodos (clic en puerto salida → clic en puerto entrada) |
| `Ctrl + C` / `Ctrl + V` | Copiar / pegar nodos seleccionados |
| `Ctrl + Z` / `Ctrl + Y` | Deshacer / rehacer |
| `Ctrl + Shift + L` | Auto-organizar nodos en el lienzo |
| `Supr` | Eliminar nodos seleccionados |
| `Ctrl + A` | Seleccionar todos los nodos |

**Movimientos avanzados:**

- **Duplicar un flujo:** selecciona un grupo de nodos, `Ctrl + C`, `Ctrl + V` y arrastra la copia a otra zona.
- **Limpiar la parrilla:** usa `Ctrl + Shift + L` para ordenar todo el lienzo con un solo comando.
- **Conexión rápida:** `Ctrl + Clic` en un puerto de salida y luego clic normal en el puerto de entrada; FloWorks dibuja la conexión automáticamente.

---

## 🎨 Personalización del entorno

FloWorks se adapta a ti, no al revés.

### Cambio de tema en caliente
Desde la barra superior, menú **Ver → Tema**, elige entre claro, oscuro u otros. La interfaz cambia **instantáneamente**, sin reiniciar ni perder el flujo de trabajo.

### Tamaño de fuente
En **Ver → Tamaño de fuente** selecciona un valor predefinido o uno personalizado. Toda la interfaz se ajusta al momento.

### Idioma
En **Ver → Idioma** selecciona el idioma deseado. FloWorks soporta **cambio en caliente**: los menús, botones y mensajes se traducen sin reiniciar la aplicación.

---

## ❗ Solución de problemas comunes

| Problema | Posible causa | Solución |
|----------|---------------|----------|
| El flujo no se ejecuta | Hay nodos sin configurar o conexiones rotas | Revisa que todos los nodos tengan parámetros válidos y que las conexiones sean entre puertos compatibles |
| La gráfica no se actualiza | El flujo está en pausa o no hay datos fluyendo | Asegúrate de haber pulsado `F5` o ▶, y de que los nodos de origen estén generando datos |
| No puedo conectar dos nodos | Los puertos son de distinto tipo | Verifica que ambos puertos sean de **datos** o ambos de **control** |
| El programa va lento con flujos grandes | Demasiados nodos o gráficas en tiempo real | Cierra paneles de análisis no usados o reduce la frecuencia de muestreo de los nodos fuente |
| El tema no cambia | Algunos widgets pueden no estar registrados | Reinicia la aplicación y vuelve a intentarlo (en versiones futuras estará resuelto) |

---

## 🧪 Ejemplos prácticos rápidos

Además del flujo senoidal inicial, prueba estos mini-proyectos para dominar FloWorks:

| Ejemplo | Nodos involucrados | Resultado esperado |
|---------|-------------------|--------------------|
| **Filtro pasa-bajos** | Generador → Filtro → Visualizador de Graficas | Verás la señal filtrada |
| **Adquisición simulada** | Generador → Analizador de THD | valor de la distorsión armonica de la señal |
| **Control manual** | Generador → Inspector de datos | tabla con los valores de la señal enviada por el generador |
| **Comparación de señales** | Dos generadores → Sumador → Visualizador de Graficas | el resultado de la operación (suma, resta, multiplicación o división) de dos ondas en una sola gráfica |

Cada uno de estos flujos se puede montar en menos de un minuto, demostrando la agilidad de FloWorks frente al código tradicional.

---

## 📚 ¿Qué sigue?

| Recurso | Descripción |
|---------|-------------|
| [🗺️ Guía de Anatomía de la Interfaz](interface-anatomy.md) | Entendimiento de la arquitectura y filosofía de la interfaz gráfica |
| [🗺️ Mapa de Código y Arquitectura](philosophy.md) | Estructura completa, managers, contratos y DPI-Awareness. |
| [🧩 Referencia Técnica de Nodos](node-reference.md) | Catálogo, `ScriptNode`, multicanal y cómo extender el sistema. |
| [🌐 Guía de Internacionalización](translation-guide.md) | Añadir idiomas, validar JSON y gestionar claves `tr()`. |
| [📦 Guía de Build Portable](guia-ejecutable-portable.md) | PyInstaller, hooks, `--onefile`, solución de errores y firma digital. |

---

!!! warning "Notas de compatibilidad y uso"
    1. **Versión de Python:** Puedes usar 3.9+, y sistemas de 64 bits.
    2. **Firewall de Windows:** Si usas hardware real (osciloscopio VISA/SCPI), permite `FloWorks.exe` en el firewall. La app muestra un diálogo personalizado si la conexión es bloqueada (el diálogo del SO no aparece en modo `--windowed`).
    3. **Atajos clave:** `F5` (ejecutar), `Ctrl+S` (guardar `.sflow`), `Ctrl+Clic` (conectar), `Espacio+clic` (paneo libre), `Ctrl+Shift+L` (auto-layout).
    4. **Preservación de datos:** El motor **nunca** aplica `flatten()` a los arrays. Trabaja con copias locales si necesitas vectorizar.