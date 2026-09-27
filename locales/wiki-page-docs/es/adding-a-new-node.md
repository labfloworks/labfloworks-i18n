---
title: Guía para Añadir un Nuevo Nodo a FloWorks
description: Tutorial paso a paso para crear, registrar e integrar nodos personalizados en el motor de flujo de FloWorks.
---

# 📘 Guía para Desarrolladores: Cómo Añadir un Nuevo Nodo a FloWorks

Esta guía describe el proceso completo para crear un nuevo tipo de nodo en FloWorks, asegurando que se integre correctamente con el motor de flujo, la interfaz de usuario, los temas visuales y el sistema de internacionalización.

---

## 📋 Tabla de Contenidos
- [📘 Guía para Desarrolladores: Cómo Añadir un Nuevo Nodo a FloWorks](#-guía-para-desarrolladores-cómo-añadir-un-nuevo-nodo-a-floworks)
  - [📋 Tabla de Contenidos](#-tabla-de-contenidos)
  - [1. Introducción a la Arquitectura](#1-introducción-a-la-arquitectura)
  - [2. Usando la Plantilla `template_node.py`](#2-usando-la-plantilla-template_nodepy)
  - [3. Paso a Paso: Creación de un Nodo Personalizado](#3-paso-a-paso-creación-de-un-nodo-personalizado)
    - [3.1. Copiar y Renombrar la Plantilla](#31-copiar-y-renombrar-la-plantilla)
    - [3.2. Definir Puertos y Etiquetas](#32-definir-puertos-y-etiquetas)
    - [3.3. Implementar la Lógica de Procesamiento](#33-implementar-la-lógica-de-procesamiento)
    - [3.4. Personalizar la Apariencia (Opcional)](#34-personalizar-la-apariencia-opcional)
    - [3.5. Añadir Parámetros Configurables (Opcional)](#35-añadir-parámetros-configurables-opcional)
    - [3.6. Hacer el nodo serializable (guardar / cargar configuraciones)](#36-hacer-el-nodo-serializable-guardar--cargar-configuraciones)
  - [4. Integración en el Sistema](#4-integración-en-el-sistema)
  - [5. Internacionalización (i18n)](#5-internacionalización-i18n)
  - [6. Temas Visuales](#6-temas-visuales)
  - [7. Lista de Verificación y Solución de Problemas](#7-lista-de-verificación-y-solución-de-problemas)
    - [✅ Lista de Verificación](#-lista-de-verificación)
    - [🐛 Problemas Comunes](#-problemas-comunes)
  - [8. Conclusión](#8-conclusión)

---

## 1. Introducción a la Arquitectura

FloWorks está construido sobre PySide6 y utiliza un modelo de nodos conectables que representan un flujo de procesamiento de señales.

---

## 2. Usando la Plantilla `template_node.py`

Para facilitar la creación de nuevos nodos, se proporciona el archivo `nodes/template_node.py`. Esta plantilla incluye:
- Soporte completo para internacionalización (conexión a `languageChanged`, método `update_language`).
- Soporte completo para temas (método `update_theme`).
- Ayuda integrada con formato HTML de tres secciones.
- Gestión de múltiples puertos de entrada/salida configurables.
- Múltiples salidas con `get_output_for_port`.
- Visualización en el plot mediante `get_display_signal`.
- Menú contextual traducible.

Se recomienda partir siempre de esta plantilla al desarrollar un nuevo nodo.

---

## 3. Paso a Paso: Creación de un Nodo Personalizado

### 3.1. Copiar y Renombrar la Plantilla
1. Copia `nodes/template_node.py` con el nombre de tu nuevo nodo, por ejemplo `nodes/mi_nodo.py`.
2. Renombra la clase de `TemplateNode` a algo descriptivo, ej. `MiNodoNode`.
3. Ajusta los imports si es necesario.

### 3.2. Definir Puertos y Etiquetas
!!! warning "Importante: Coincidencia de Nombres"
    Los nombres de puerto en `PORTS`, `PORT_LABELS` y las claves del diccionario devuelto por `execute_program` deben ser **exactamente iguales** (incluyendo mayúsculas/minúsculas). La plantilla ahora incluye un mapeo de alias (`'data_in'` → primer puerto izquierdo) para mayor robustez.

Edita el diccionario `PORTS` en la parte superior del archivo. Cada entrada tiene el formato:
```python
"nombre_puerto": ("lado", fracción)
```
- **Lados posibles:** `"left"`, `"right"`, `"top"`, `"bottom"`.
- **Fracción:** valor entre `0.0` y `1.0` que indica la posición a lo largo del lado.

**Ejemplo para un nodo con una entrada y dos salidas:**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
El diccionario `PORT_LABELS` contiene el texto que aparecerá junto a cada puerto. Se recomienda usar claves de traducción en lugar de texto fijo (ver sección Internacionalización).

### 3.3. Implementar la Lógica de Procesamiento
El método clave es `execute_program(self, input_data)`. Este método es invocado por el motor de flujo cuando el nodo recibe datos.

**`input_data` puede ser:**
- `None` si no hay entrada.
- Una tupla `(x, y)` para señales temporales.
- Un array 1D.
- Un diccionario `{nombre_puerto: datos}` en nodos con múltiples entradas.

**Valor de retorno:**
- Para nodos con una única salida, devuelve directamente los datos (ej. tupla `(x, y)`).
- Para nodos con múltiples salidas, devuelve un diccionario donde las claves coinciden con los nombres de los puertos de salida definidos en `PORTS`.

```python
def execute_program(self, input_data):
    # Procesar input_data y generar resultados
    resultado_magnitud = (freq, mag)
    resultado_fase = (freq, phase)
    return {
        "magnitude": resultado_magnitud,
        "phase": resultado_fase
    }
```

!!! tip "Nota sobre nombres de puerto genéricos"
    El motor de flujo puede pasar ocasionalmente un diccionario con claves como `'data_in'` en lugar del nombre real del puerto (especialmente si el usuario no hizo clic exacto en el círculo). La plantilla ya incluye código para manejar este caso:
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    Esto evita que el nodo falle por un error de conexión impreciso.

La plantilla ya incluye un ejemplo comentado. Además, implementa `get_output_for_port(self, port_name)` para que el motor pueda enrutar cada salida:
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. Personalizar la Apariencia (Opcional)
El método `paint()` dibuja el fondo, título, estado y cualquier texto adicional. Puedes modificar:
- Los colores (se actualizan automáticamente con `update_theme`).
- El texto de estado (usando el atributo `self._status`).
- Información de resumen (ej. pico de magnitud).

La plantilla muestra un ejemplo básico.

### 3.5. Añadir Parámetros Configurables (Opcional)
Si tu nodo requiere parámetros ajustables por el usuario (ej. tamaño de ventana, frecuencia de corte), puedes:
1. Agregar atributos en `__init__` (ej. `self.window_size = 512`).
2. Crear un diálogo de configuración (hereda de `QDialog`).
3. Conectar el diálogo en `open_config_dialog()` (método ya presente en la plantilla).
4. Actualizar los parámetros desde el diálogo y llamar a `self.update()`.

### 3.6. Hacer el nodo serializable (guardar / cargar configuraciones)
Para que el nodo pueda guardar y recuperar sus parámetros al copiar/pegar, deshacer/rehacer, o al usar los comandos Guardar/Abrir del menú Archivo, debe heredar del mixin de serialización y declarar sus atributos.

1. Importa el mixin en tu archivo:
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. Cambia la herencia de la clase para incluirlo antes de `QGraphicsObject`:
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
    ```
3. Define la lista `SERIALISABLE` a nivel de clase, con los nombres de los atributos que quieres persistir. Solo admite tipos simples (`int`, `float`, `str`, `bool`), listas, diccionarios, o arrays NumPy (estos últimos se almacenan automáticamente como archivos `.npy` dentro del `.sflow`).
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frecuencia', 'amplitud', 'configuracion']
    ```
4. Asegúrate de que esos atributos se inicialicen en `__init__`:
    ```python
    self.frecuencia = 1000.0
    self.amplitud = 1.0
    self.configuracion = {'tipo': 'seno', 'fase': 0}
    ```

Con esto, no necesitas escribir métodos `serialize`/`deserialize`; el mixin se encarga automáticamente de guardar y recuperar los valores.

Si tu nodo requiere lógica adicional al cargar (por ejemplo, reconectar un instrumento hardware), puedes sobrescribir `deserialize` llamando primero al método padre:
```python
def deserialize(self, data):
    super().deserialize(data)   # restaura los atributos de SERIALISABLE
    self._iniciar_dispositivo()
```

---

## 4. Integración en el Sistema

Una vez creado el archivo del nodo, solo debes pegarlo en la carpeta `nodes` para que aparezca en la interfaz y funcione con el resto del sistema.

---

## 5. Internacionalización (i18n)

Todos los textos visibles deben ser traducibles mediante `tr("clave", default="...")`. La plantilla ya lo implementa. Debes agregar las claves correspondientes en los archivos JSON dentro de `locales/`.

**Estructura recomendada:**
```json
{
   "nodes": {
     "mi_nodo": {
       "title": "Mi Nodo",
       "tooltip": "Descripción emergente",
       "ports": {
         "input": "Entrada",
         "output1": "Salida 1",
         "output2": "Salida 2"
      },
       "status": {
         "no_data": "Sin datos",
         "ready": "Listo"
      },
       "menu": {
         "show_output": "Mostrar salida",
         "configure": "Configurar..."
      },
       "help_title": "Ayuda - Mi Nodo",
       "help_html": "<h3>🎛️ Filter Node</h3>\n<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_mi_nodo": "Mi Nodo"
  }
}
```

La ayuda HTML sigue el formato de tres secciones común a todos los nodos (descripción específica + "Cómo pensar en el sistema" + "Atajos y trucos"). La plantilla ya incluye la estructura en `get_help_text()`.

---

## 6. Temas Visuales

El método `update_theme(self, theme)` recibe un diccionario con los colores definidos por el tema actual. La plantilla actualiza automáticamente:
- Fondo del nodo (`node_normal_bg`)
- Borde (`node_selected_border`)
- Color de título y texto (`node_normal_text`)
- Colores de puertos (`port_circle`, `port_outline`, `port_inline`, `port_text`)

Asegúrate de que en `MainWindow` (o `ThemeUpdater`) se llame a `node.update_theme()` para cada nodo cuando cambie el tema.

---

## 7. Lista de Verificación y Solución de Problemas

### ✅ Lista de Verificación
- [ ] El nodo se crea correctamente desde la barra de herramientas.
- [ ] Los puertos se muestran en las posiciones esperadas y son detectables para conexiones (`Ctrl+clic`).
- [ ] Al recibir datos de entrada, `execute_program` es llamado y se procesa la señal.
- [ ] Las salidas se propagan correctamente a nodos conectados.
- [ ] El menú contextual permite cambiar el canal de visualización (si hay múltiples salidas).
- [ ] Al hacer clic en el nodo, se grafica la señal seleccionada en el plot widget.
- [ ] El doble clic abre la ayuda con el formato adecuado.
- [ ] El idioma cambia correctamente (textos de título, puertos, menús).
- [ ] El tema cambia correctamente (colores del nodo y puertos).
- [ ] Copiar/pegar funciona sin errores.

!!! tip "Conexión precisa de puertos"
    Al conectar nodos, asegúrate de hacer clic exactamente sobre el círculo del puerto de destino. Si haces clic en el cuerpo del nodo, el sistema usará un nombre genérico (`'data_in'`). La plantilla ahora tolera estos nombres, pero es una buena práctica conectar directamente al círculo para garantizar el enrutamiento correcto de múltiples salidas.

### 🐛 Problemas Comunes

| Síntoma | Posible Causa | Solución |
|---------|---------------|----------|
| La flecha de conexión no se ancla al puerto. | El círculo del puerto no tiene `setData(0, port_name)` o `get_port_scene_pos` no está implementado. | Verificar que en `_create_ports` se haga `circle.setData(0, port_name)` y que `get_port_scene_pos` use ese nombre. |
| Las salidas no llegan a los nodos conectados. | `execute_program` no devuelve un diccionario (para múltiples salidas) o `get_output_for_port` no está implementado. | Asegurar que `execute_program` devuelva `{nombre_puerto: datos}` y que `get_output_for_port` devuelva el valor correspondiente. |
| Al hacer clic en el nodo no se grafica nada. | `get_display_signal` no devuelve una tupla `(x, y)` válida o `display_channel` no coincide con una salida existente. | Revisar que `get_display_signal` use el canal seleccionado y que los datos sean arrays NumPy. |
| Los textos no se actualizan al cambiar idioma. | No se conectó la señal `languageChanged` o `update_language` no actualiza los elementos. | Verificar la conexión en `__init__`: `language_manager.languageChanged.connect(self.update_language)`. |
| El tema no se aplica. | No se llama a `update_theme` al crear el nodo o al cambiar tema. | En `MainWindow`, después de crear el nodo, invocar `node.update_theme(self.theme_manager.current_theme())`. |
| La flecha apunta al centro del nodo. | Se hizo clic en el cuerpo en lugar del círculo, o el nombre no coincide con `PORTS`. | Haz clic directamente sobre el círculo. Verifica que `get_port_scene_pos` tenga el mapeo de alias. |
| `NameError: name 'self' is not defined` al importar. | Se declararon atributos de instancia fuera de `__init__`. | Todos los atributos como `self.mi_parametro` deben definirse dentro de `__init__`. |
| Parámetros se pierden al copiar/abrir `.sflow`. | El nodo no hereda de `SerializableMixin` o no definió `SERIALISABLE`. | Implementar el paso 3.6 de esta guía. |

---

## 8. Conclusión

Siguiendo esta guía y utilizando la plantilla `template_node.py`, podrás añadir nuevos nodos a FloWorks de manera eficiente y coherente con el resto del sistema. Recuerda siempre mantener la compatibilidad con i18n y temas para una experiencia de usuario profesional.

¡Anímate a contribuir con tus propios nodos!
