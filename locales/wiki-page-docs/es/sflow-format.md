---
title: Formato de Archivo .sflow
description: Especificación técnica, estructura interna y guía de uso del estándar de intercambio de FloWorks
---

# 📄 Formato de Archivo `.sflow`

El formato `.sflow` es el estándar nativo de intercambio y persistencia de **FloWorks**. Permite empaquetar un flujo de trabajo completo en un único archivo que incluye la topología del grafo, los parámetros de los nodos, los datos procesados y notas adhesivas, facilitando compartir, archivar o reproducir experimentos de forma determinista.

---

## 📦 ¿Qué es un archivo `.sflow`?

Un archivo `.sflow` es, en esencia, un **archivo ZIP renombrado**. Al cambiar su extensión a `.zip`, puedes inspeccionar su contenido con cualquier gestor de archivos o herramienta de línea de comandos.

Su estructura interna mínima consta de:

| Componente | Descripción |
|------------|-------------|
| `diagram.json` | Manifiesto principal: define nodos, conexiones, vista, notas adhesivas y metadatos de serialización. |
| `data/` | Carpeta con los datos de cada nodo en formato `.npy` (arrays binarios de NumPy). |
| `metadata.json` *(opcional)* | Información complementaria: autor, versión de FloWorks, descripción y etiquetas. |

=== "🌳 Estructura Visual"
    ```text
    mi-flujo.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (opcional, para ScriptNode persist)
    ```

---

## 🧩 `diagram.json` – El Corazón del Flujo

Este archivo JSON describe la topología completa, la posición de los elementos en el lienzo y el estado de la vista al momento de guardar.

### Ejemplo Mínimo
```json
{
  "nodes": [
    {
      "id": "n1",
      "type": "OscilloscopeNode",
      "pos": [150, 200],
      "params": { "channel": "primary", "simulation": false }
    },
    {
      "id": "n2",
      "type": "GraphExporterNode",
      "pos": [450, 200],
      "params": { "theme": "dark", "export_format": "png" }
    }
  ],
  "connections": [
    {
      "from": "n1",
      "to": "n2",
      "from_port": "out",
      "to_port": "data_in"
    }
  ],
  "viewport": { "x": 0, "y": 0, "scale": 1.0 },
  "stickers": [
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "Revisar umbral", "user_modified": true }
  ]
}
```

### Campos Principales
| Campo | Tipo | Descripción |
|-------|------|-------------|
| `nodes` | `Array` | Lista de objetos `{id, type, pos, params}`. `type` debe coincidir con `node_registry.py`. |
| `connections` | `Array` | Lista de conexiones `{from, to, from_port, to_port}`. Los puertos son cadenas, no índices. |
| `viewport` | `Object` | `(x, y, scale)` para restaurar exactamente la posición y zoom del lienzo. |
| `stickers` | `Array` | Notas adhesivas serializadas con coordenadas normalizadas a 96 dpi. |

!!! tip "Serialización de Nodos"
    Los parámetros específicos de cada nodo se gestionan mediante `SerializableMixin`. Solo se guardan los atributos declarados en `SERIALISABLE = [...]`. Los nodos no registrados en el sistema se omiten automáticamente durante la carga.

---

## 💾 `data/` – Datos Procesados y Arrays NumPy

Cuando un flujo se ejecuta, los nodos pueden almacenar sus resultados en archivos `.npy` dentro de esta carpeta.

- El nombre del archivo suele coincidir con el `id` del nodo o con referencias internas.
- Los arrays se almacenan en formato binario de NumPy, **preservando estrictamente la dimensionalidad original** (1D, 2D, 3D, etc.). El motor nunca aplica `flatten()`.
- En `diagram.json`, los datos se referencian con el prefijo `__npy__:`:
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- Si un nodo no produce datos o está configurado para no persistirlos, el archivo correspondiente puede omitirse.

??? note "Compatibilidad Externa"
    Los archivos `.npy` son universales en el ecosistema Python. Puedes leerlos fuera de FloWorks con:
    ```python
    import numpy as np
    datos = np.load("data/node_1.npy")
    print(datos.shape)
    ```

---

## 🏷️ `metadata.json` (Opcional)

Contiene información descriptiva que no afecta la ejecución, ideal para trazabilidad y gestión de proyectos:

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "Análisis de vibraciones en motor trifásico",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["ingeniería", "vibraciones", "FFT", "multicanal"]
}
```

---

## 🔄 Proceso de Guardado y Carga

FloWorks implementa un mecanismo robusto para garantizar la integridad de los datos:

1. **Guardado:** 
   - Se recorre el grafo y se serializan nodos vía `SerializableMixin`.
   - Los arrays se extraen a `data/` y se referencian en JSON con `__npy__:`.
   - Todo se empaqueta en un ZIP con extensión `.sflow`.
2. **Carga Segura:** 
   - Se crea un **backup temporal en memoria** del diagrama actual.
   - Se extrae y parsea el nuevo `.sflow`.
   - Si ocurre cualquier error (JSON inválido, nodos faltantes, corrupción de `.npy`), **se restaura automáticamente el backup** sin pérdida de trabajo.
3. **ScriptNode Especial:** 
   - Guarda `script`, `params`, `dynamic_inputs`, `dynamic_outputs`, `persist` y `python_path`.
   - Al cargar, recompila el código, reconstruye los puertos dinámicos y restaura el estado `persist` automáticamente.
4. **StickyNotes & DPI:** 
   - Las coordenadas y tamaños se normalizan a **96 dpi** al guardar.
   - Al cargar, se escalan al DPI del monitor actual, garantizando consistencia visual entre diferentes resoluciones.

---

## 🛠️ Uso Externo y Automatización

El formato `.sflow` está diseñado para ser transparente y programático. Puedes leerlo o generarlo desde scripts externos:

=== "🐍 Python (Lectura)"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("mi-flujo.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))
        
        datos_n1 = np.load(z.open("data/node_1.npy"))
        print(f"Nodos: {len(graph['nodes'])}")
        print(f"Datos: {datos_n1.shape}")
    ```

=== "📤 Python (Creación Básica)"
    ```python
    import zipfile
    import json
    import numpy as np

    graph = {
        "nodes": [{"id": "gen", "type": "GeneratorNode", "pos": [100, 100], "params": {}}],
        "connections": [],
        "viewport": {"x": 0, "y": 0, "scale": 1.0}
    }
    
    with zipfile.ZipFile("nuevo.sflow", "w", zipfile.ZIP_DEFLATED) as z:
        z.writestr("diagram.json", json.dumps(graph, indent=2))
        z.writestr("data/gen.npy", np.array([1.0, 2.0, 3.0]))
    ```

---

## 🔮 Compatibilidad y Extensibilidad Futura

El formato `.sflow` sigue los principios de **diseño extensible y retrocompatible**:

- ✅ **Nuevas secciones:** Futuras versiones pueden añadir carpetas como `thumbnails/`, `logs/` o `plugins/` sin romper cargadores antiguos.
- ✅ **Campos opcionales:** El parser ignora claves desconocidas en `diagram.json`, permitiendo añadir metadatos experimentales.
- ✅ **Versionado:** El campo `floworks_version` en `metadata.json` permite a la aplicación aplicar migraciones automáticas si el formato evoluciona.

!!! warning "Regla de Oro"
    Nunca modifiques manualmente `diagram.json` mientras la aplicación está abierta. El sistema depende de la consistencia entre la topología, los arrays y el estado de la vista. Usa siempre los flujos de guardado/carga nativos.

---

## 📚 Recursos Relacionados
- [🗺️ Mapa de Código y Arquitectura](architecture-ii.md) → Cómo `file_io.py` y `SerializableMixin` gestionan el formato.
- [📦 Guía de Build Portable](guia-ejecutable-portable.md) → Empaquetado y rutas seguras para recursos.
- [🧩 Referencia de Nodos](node-reference.md) → Contratos de serialización por tipo de nodo.