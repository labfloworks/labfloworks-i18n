# Filosofia

## Visión general

FloWorks es una aplicación de escritorio que le permite crear cadenas de procesamiento de señales mediante diagramas de flujo visuales.  
Arrastre, conecte y configure nodos; el resultado se calcula y se muestra en tiempo real.  
Trabaje con señales simuladas o conecte instrumentos reales (osciloscopios, generadores, multímetros LCR) sin necesidad de escribir código, aunque dispone de un potente entorno de scripting si desea ampliar la funcionalidad.

---

## Características principales

- **Diagramas interactivos** – Construya su flujo de trabajo uniendo nodos con líneas que representan el flujo de datos.
- **Procesamiento en tiempo real** – Cada modificación se refleja inmediatamente en las gráficas y visualizaciones.
- **Simulación y hardware real** – Genere señales de prueba o capture datos directamente desde instrumentos de laboratorio.
- **Nodo de script avanzado** – Incorpore su propio código Python con ayuda de autocompletado, parámetros dinámicos editables y memoria persistente entre ejecuciones.
- **Visualización profesional** – Señales, espectros, espectrogramas y gráficos de alta calidad listos para exportar.
- **Multilingüe** – La interfaz detecta el idioma del sistema y permite cambiar entre español, inglés y otros idiomas en cualquier momento.
- **Temas visuales** – Modo oscuro, claro y de alto contraste para adaptarse a sus preferencias o necesidades de accesibilidad.
- **Gestión completa de proyectos** – Guarde su trabajo en archivos `.sflow` y recupérelos exactamente como los dejó, con deshacer y rehacer ilimitados.

---

## Cómo trabajar con FloWorks

### Nodos
Un nodo es una pieza del procesamiento. Se organizan en tres categorías:

- **Fuentes** – Insertan señales al inicio del flujo. Por ejemplo, un osciloscopio (real o simulado), un generador de funciones o una operación matemática.
- **Procesamiento** – Transforman los datos. Sumas, restas, condicionales, filtros… inclusive un nodo especial para escribir sus propios scripts en Python.
- **Sumideros** – Muestran o exportan los resultados. El visualizador gráfico y el exportador de gráficos profesionales son los más utilizados.

### Conexiones
Las uniones entre nodos se dibujan como curvas suaves o líneas ortogonales. Una animación de flujo le indica en todo momento la dirección de los datos. El sistema organiza automáticamente los cables para que no se solapen.

### Visualización
Cada vez que un nodo produce una señal, esta puede verse en el panel de gráficas integrado. Puede explorar diferentes representaciones (forma de onda, espectro, espectrograma) y ajustar la escala con el ratón.

---

## Nodos destacados
Son los nodos minimos indispensables, necesarios para que la filosofía del programa tenga sentido.

### Nodo generador de señales
Fuente de señales que puede generar simulaciones personalizadas de formas de onda a gusto del usuario. Permite desde un menú contextual seleccionar o digitar la forma de onda requerida.

### Nodo de script
Un entorno de programación completo dentro del diagrama:

- **Editor con resaltado de sintaxis**, autocompletado y consola de errores.
- **Parámetros dinámicos** – Defina variables editables desde el panel del nodo sin modificar el código.
- **Puertos configurables** – Añada entradas y salidas adicionales directamente desde el editor.
- **Estado persistente** – Guarde valores entre ejecuciones; todo se almacena junto con el proyecto.

### Exportador de gráficos
Nodo sumidero que genera imágenes de alta calidad para informes o publicaciones. Permite configurar tamaño, resolución, formato, entre otros.

---

## Personalización

- **Idioma** – La aplicación detecta automáticamente el idioma del sistema y guarda su preferencia. Puede cambiarlo desde el menú sin reiniciar.
- **Apariencia** – Elija entre tema oscuro, claro o de alto contraste según la luz ambiente o sus necesidades visuales.

---

## Proyectos y archivos

Guarde su diagrama completo en un archivo `.sflow`.  
Al abrirlo recuperará todos los nodos, conexiones, scripts, parámetros y configuraciones de visualización.  
Las acciones de deshacer y rehacer le permiten experimentar sin miedo a perder el trabajo anterior.

---

FloWorks está diseñado para que se concentre en el análisis de señales y no en los detalles técnicos de la implementación. Arrastre, conecte y descubra.