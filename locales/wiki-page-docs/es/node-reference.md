---
title: Referencia Técnica de Nodos
description: Catálogo actualizado, contratos de extensión y capacidades avanzadas del sistema de nodos de FloWorks
---

# 🧩 Referencia Técnica de Nodos

FloWorks no depende de un catálogo estático. Utiliza un **sistema de registro dinámico** basado en contratos claros. Esto permite ampliar la plataforma sin tocar el motor topológico. A continuación se detalla el catálogo implementado, las capacidades técnicas reales y el protocolo para extenderlo de forma segura.

---

## 📂 Categorías del Núcleo

=== "📦 Vista por Capas"
    <div class="grid cards" markdown>

    - **📥 Fuentes/Entrada**
      Generan o capturan señales iniciales. Soportan simulación integrada, hardware real (VISA/SCPI) y modo multicanal.
    - **⚙️ Procesamiento**
      Transforman, combinan o analizan datos. Preservan dimensionalidad e interpolan automáticamente cuando es necesario.
    - **🔀 Control/Flujo**
      Bifurcan, iteran o condicionan la ejecución. Incluyen soporte nativo para señales de activación.
    - **🐍 Scripting/Avanzado**
      Ejecutan código Python dinámico con puertos paramétricos (`# @param`), puertos dinámicos (`# @input`/`# @output`) y persistencia de estado (`persist`).
    - **🔌 Hardware/Instrumentación**
      Interfaces para osciloscopios, medidores LCR y generadores.
    - **📤 Salida/Exportación**
      Visualizan, exportan o archivan resultados. Soportan temas visuales, perfiles de usuario y formato profesional (PNG/PDF/SVG).

    </div>

---

## 📋 Catálogo Técnico Implementado

| Nodo | Tipo | Responsabilidad Principal | Características Clave |
|------|------|---------------------------|------------------------|
| `SumNode` | Procesamiento | Operador aritmético (+, -, *, /) para dos entradas. | Interpola automáticamente señales de distinta resolución (FFTs). Preserva dimensionalidad. |
| `RhombusNode` | Control | Condicional (bifurcación Si/No). | Dos puertos de salida. Evalúa condición por umbral o lógica booleana. |
| `TriggerNode` | Control | Iterador/acumulador con activación externa. | Recibe `(x, y, "trigger")`. Acumula hasta N iteraciones y emite resultado apilado/promediado. |
| `ScriptNode` | Avanzado | Entorno de scripting Python integrado. | QScintilla, autocompletado, `# @param`, puertos dinámicos, `persist`, plantillas, consola de errores, intérprete externo con timeout. |
| `OscilloscopeNode` | Hardware | Captura desde osciloscopios (SDS) o medidores LCR. | Modo simulación, diálogo de firewall integrado, **soporte multicanal** (`out_primary`, `out_secondary`), menú "Mostrar canal". |
| `GeneratorNode` | Fuente | Envía señales a generadores (SDG) o simula salidas. | Configuración de modulación/sweep, diálogo de simulación integrado. |
| `GraphExporterNode` | Salida | Exportador de gráficos profesionales. | Configuración por doble clic, ejes personalizados, temas, perfiles guardados, etc. |

---

## 🔍 ScriptNode: Capacidades Esenciales

> **🐍 Entorno de Scripting Integrado**
>
> - **Editor de código integrado:** Resaltado de sintaxis básico, numeración de líneas y plegado de código.
> - **Panel de parámetros dinámicos:** Directivas `# @param NOMBRE : tipo = valor` inyectan controles editables (spinbox, campo de texto, etc.) en el panel lateral.
> - **Puertos dinámicos:** `# @input nombre` y `# @output nombre` crean puertos en tiempo real. El script recibe un diccionario `inputs` y devuelve `outputs`.
> - **Persistencia de estado:** Diccionario global `persist` que mantiene valores entre ejecuciones.
> - **Plantillas e Import/Export:** Menú desplegable con scripts base. El usuario puede guardar sus scripts en `nodes/script_node/templates/` o importar/exportar archivos `.py` externos.
> - **Consola de errores integrada:** Muestra fallos de sintaxis/ejecución con la línea exacta señalada en el editor.
> - **Ayuda e i18n:** Tooltips contextuales, botón `?` con guía rápida, y todos los textos usan `tr()` para traducción.
> - **Intérprete externo con timeout:** Ruta configurable (`# @python_path` o botón "Examinar…"). Ejecución aislada con límite de tiempo y fallback al intérprete interno.
> - **Serialización completa:** Guarda script, parámetros, puertos dinámicos y estado `persist`. Al cargar un `.sflow`, reconstruye automáticamente puertos y parámetros.

---

## 📚 Recursos Relacionados

- [📖 Mapa de Código y Arquitectura](architecture-ii.md) → Responsabilidades por módulo y flujos de trabajo.
- [🌐 Guía de Internacionalización (i18n)](i18n.md) → Cómo añadir idiomas y gestionar claves `tr()`.
- [🛠️ Añadir un Nuevo Nodo (Tutorial)](adding-a-new-node.md) → Paso a paso con ejemplos prácticos.
- [📦 Guía de Build y Distribución](build.md) → Empaquetado PyInstaller, hooks y firmas digitales.

---

💡 **¿Falta un nodo en este catálogo?**  
FloWorks está diseñado para ser extensible. Si necesitas un nodo que no existe, créalo siguiendo el contrato de `BaseNode` y regístralo. La comunidad y el futuro marketplace ampliarán continuamente el ecosistema sin romper compatibilidad.