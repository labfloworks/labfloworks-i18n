---
title: Arquitectura de FloWorks
description: Visión general de los componentes y funcionamiento interno para el usuario final
---

# Arquitectura de FloWorks – Visión para el usuario

FloWorks es una aplicación de escritorio que le permite construir cadenas de procesamiento de señales mediante diagramas de flujo. Conecta bloques (nodos) en un lienzo interactivo y vea los resultados en tiempo real. Para hacer esto posible, la aplicación está organizada en varios módulos que trabajan juntos. A continuación se explica, sin detalles técnicos, qué hace cada parte y cómo se relacionan entre sí.

---

## Estructura general

La aplicación se compone de las siguientes áreas funcionales:

| Área | ¿Qué hace? |
|------|------------|
| **Inicio y ventana principal** | Arranca el programa, muestra la ventana, los menús y coordina todas las acciones del usuario. |
| **Motor de ejecución** | Calcula el orden en que deben ejecutarse los nodos, detecta dependencias y bucles, y transmite los datos de un nodo a otro. |
| **Escena y diagrama** | Gestiona el lienzo donde coloca los nodos, las conexiones entre ellos, las notas adhesivas y las acciones de deshacer/rehacer. |
| **Nodos y procesamiento** | Contiene todos los tipos de bloques que puede usar: fuentes de señal, operaciones matemáticas, scripts personalizados, exportación de gráficos, etc. |
| **Conectores visuales** | Dibuja las líneas que unen los nodos (curvas suaves u ortogonales), las anima para mostrar el flujo de datos y evita que se solapen. |
| **Interfaz de usuario** | Incluye la vista del diagrama (zoom, desplazamiento), la barra de herramientas, la tabla de parámetros, los paneles de análisis (estadísticas, cursores) y los diálogos de configuración. |
| **Soporte para hardware real** | Permite la comunicación con instrumentos de laboratorio (osciloscopios, generadores, multímetros LCR) para capturar o generar señales reales. |
| **Exportación de gráficos** | Genera imágenes de alta calidad (PNG, PDF, SVG) con total personalización visual. |
| **Temas y apariencia** | Cambia el aspecto de toda la aplicación (oscuro, claro, alto contraste) y permite ajustar el tamaño de letra. |
| **Idiomas** | Traduce toda la interfaz a varios idiomas y permite cambiar el idioma al instante. |
| **Gestión de proyectos** | Guarda y abre archivos `.sflow` con todo el diagrama, incluyendo configuraciones, scripts y resultados. |
| **Pruebas y diagnóstico** | Herramientas internas para verificar que todo funciona correctamente (no visibles para el usuario final). |

---

## Cómo funciona por dentro

### Arranque y ventana principal
Al abrir FloWorks, se configura el entorno gráfico, se detecta la densidad de píxeles de su pantalla (para que todo se vea nítido en monitores 4K o normales) y se muestra la ventana principal. Esta ventana centraliza todos los elementos: el área de dibujo, los menús, la barra de herramientas y los paneles laterales.

### Motor de flujo
Cuando usted pulsa "Ejecutar" (o presiona F5), un motor interno recorre todos los nodos en el orden correcto, respetando las conexiones. Sabe qué nodos dependen de otros y evita los ciclos infinitos. Soporta que un nodo reciba varias entradas con nombre y que produzca múltiples salidas. Los datos viajan entre nodos sin perder su estructura original.

### Escena del diagrama
El lienzo donde construye sus diagramas es una escena inteligente:

- Permite añadir nodos, moverlos, conectarlos y seleccionarlos.
- Soporta deshacer y rehacer ilimitados para cualquier acción.
- Incluye notas adhesivas redimensionables que puede colocar libremente y que se guardan con el proyecto.
- Dispone de un organizador automático que recoloca los nodos ordenadamente (con Ctrl+Shift+L).
- Al guardar, todo el diagrama se empaqueta en un archivo `.sflow` que contiene las descripciones de los nodos, las conexiones, las notas y los datos numéricos asociados.

### Conectores
Las líneas que unen los nodos se dibujan como curvas suaves o trayectorias ortogonales. Una suave animación de puntos o guiones le indica la dirección del flujo. Un gestor de carriles evita que varias conexiones entre los mismos nodos se amontonen; las separa automáticamente para que todo sea legible.

### Tipos de nodos
Los nodos son las piezas fundamentales. Se agrupan en tres categorías:

- **Fuentes** – Generan señales. Pueden simular ondas (senoidal, cuadrada, etc.) o leer datos reales desde un osciloscopio o multímetro conectado. Soportan múltiples canales simultáneos (por ejemplo, impedancia y fase desde un LCR).
- **Procesamiento** – Transforman los datos. Incluyen operaciones aritméticas (suma, resta, multiplicación, división), decisiones condicionales (bifurcación Si/No) y un potente nodo de script que le permite escribir su propio código Python con ayudas visuales.
- **Sumideros** – Muestran o exportan los resultados. El más común es el visualizador gráfico (osciloscopio virtual), pero también existe un exportador de gráficos de calidad profesional.

Cada nodo tiene puertos de entrada (izquierda/arriba) y de salida (derecha/abajo). Al conectar un puerto de salida con uno de entrada, la señal fluye entre ellos.

#### Nodo de script avanzado
El nodo de script merece mención especial. Está pensado para usuarios avanzados que quieran añadir su propio procesamiento sin salir de FloWorks. Ofrece:

- Un editor con resaltado de sintaxis, autocompletado y numeración de líneas.
- La posibilidad de definir parámetros editables desde el panel del nodo sin tocar el código (por ejemplo, un valor numérico que luego se usa en el script).
- Puertos de entrada y salida dinámicos: añadiendo comentarios especiales en el script, puede crear nuevos conectores.
- Memoria persistente: una variable especial (`persist`) que conserva su valor entre ejecuciones, útil para acumuladores o máquinas de estado.
- Plantillas de scripts ya preparadas y la opción de guardar las suyas propias.
- Un sistema de ayuda integrado y una consola que muestra errores de ejecución.

### Interfaz de usuario
Además del lienzo, la interfaz incluye:

- Una **barra de herramientas** con todos los nodos organizados por categorías, menús de idioma, tema y tamaño de fuente, y acceso al visor de registros.
- Una **tabla de parámetros** que muestra información de los nodos seleccionados y resalta posibles incompatibilidades (como intentar operar con señales de distinta longitud).
- **Paneles de análisis** acoplables: estadísticas (máximo, mínimo, valor eficaz), cursores A/B para medir diferencias, y un punto de mira con marcador de pico.
- Un **diálogo de bienvenida** que se adapta a la resolución de su pantalla y le ofrece opciones iniciales.

### Conexión con instrumentos reales
Si dispone de hardware compatible (osciloscopios Siglent SDS, multímetros LCR, generadores SDG), FloWorks puede comunicarse con ellos mediante el protocolo estándar VISA/SCPI. La configuración se realiza desde paneles específicos dentro de la aplicación. Cuando capture una señal multicanal (por ejemplo, magnitud y fase de un LCR), el nodo fuente empaqueta todos los canales y usted puede elegir cuál visualizar con un simple menú contextual.

### Exportación de gráficos profesionales
El nodo exportador de gráficos le permite generar imágenes listas para informes o publicaciones. Al hacer doble clic sobre él, se abre un diálogo con múltiples opciones: puede personalizar colores, tipos de línea, etiquetas, escalas, elegir entre formatos PNG, PDF o SVG, y guardar sus preferencias como perfiles reutilizables.

### Personalización visual
FloWorks incluye varios temas (oscuro, claro, alto contraste) que cambian la apariencia de toda la interfaz al instante, sin reiniciar. Además, puede ajustar el tamaño de letra global desde el menú (Información → Tamaño de fuente) y todos los elementos se redimensionan en consecuencia, incluyendo los textos dentro de los nodos, las notas adhesivas y los gráficos.

### Sistema de idiomas
La aplicación detecta automáticamente el idioma de su sistema al iniciar por primera vez y guarda su preferencia. Puede cambiar el idioma en cualquier momento desde el menú; todos los textos, menús y ayudas se actualizan sobre la marcha.

### Proyectos y archivos `.sflow`
Todo su trabajo se guarda en un único archivo con extensión `.sflow`. Este archivo contiene el diagrama completo: nodos, conexiones, notas, configuraciones, scripts y los datos numéricos generados. Puede compartirlo con otros usuarios; al abrirlo en otro equipo, las notas y los nodos se reescalan automáticamente para adaptarse a la densidad de píxeles de esa pantalla.

---

## Flujos de trabajo típicos

1. **Crear un diagrama sencillo**  
   Seleccione un nodo fuente (p. ej., Generador) y un nodo Visualizador desde la barra de herramientas.  
   Conecte la salida del generador a la entrada del visualizador (Ctrl+clic en el puerto de salida, luego clic en el de entrada).  
   Pulse F5 para ejecutar. Verá la señal en la gráfica.

2. **Usar un script personalizado**  
   Añada un nodo Script.  
   Escriba su código Python en el editor; puede definir parámetros editables y puertos extra.  
   Conecte sus entradas y salidas como cualquier otro nodo.  
   Ejecute el flujo; el script se procesa con sus datos.

3. **Capturar datos de un osciloscopio real**  
   Conecte el instrumento y configure la comunicación desde el panel del nodo Osciloscopio.  
   El nodo adquiere la señal y la entrega por sus puertos de salida (uno por canal).  
   Conecte esos puertos a otros nodos de procesamiento o al visualizador.

4. **Exportar un gráfico para un informe**  
   Conecte la señal deseada a un nodo Exportador de gráficos.  
   Seleccione en el nodo (clic derecho) para configurar el aspecto visual del gráfico. 
   También se pueden cargar/guardar perfiles para agilizar obtención de graficos listos para informes obteniendo el archivo de imagen en la extensión elegida.

---

## Para qué sirve todo esto

Esta arquitectura está pensada para que usted pueda concentrarse en el análisis de señales sin preocuparse de cómo se organiza internamente el programa. Cada componente tiene una función clara y trabaja en conjunto para ofrecerle una experiencia fluida, desde la simulación hasta la instrumentación real, pasando por la personalización visual y la exportación de resultados. 

Si alguna vez necesita ampliar las capacidades de FloWorks (por ejemplo, añadiendo nuevos tipos de nodos o conectando un instrumento diferente), sepa que existe una estructura modular que lo permite, aunque ese es terreno para desarrolladores. Como usuario final, disfrute de la flexibilidad que le proporciona este diseño.