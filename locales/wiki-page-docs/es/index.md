---
title: FloWorks
description: Laboratorio visual universal para procesamiento de señales, instrumentación científica y automatización.
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## Laboratorio visual universal para señales, instrumentación e IA

Procesamiento científico • DSP • VISA/SCPI • Automatización • Machine Learning

![Captura de FloWorks](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[Primeros pasos con FloWorks](getting-started.md){ .md-button }
[Anatomía de la interfaz](interface-anatomy.md){ .md-button .md-button--primary }
[Filosofía](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## ¿Qué es FloWorks?
FloWorks es un **laboratorio visual de código abierto** (Python + PySide6) donde construyes sistemas conectando bloques (nodos) en lugar de escribir líneas de código.

Imagina un lienzo digital donde unes generadores de señales, filtros matemáticos, controladores de hardware (VISA/SCPI) y modelos de Inteligencia Artificial mediante cables virtuales. Todo se basa en el **flujo de datos**: conectas la salida de un bloque con la entrada de otro para procesar información, automatizar equipos o analizar resultados en tiempo real.

Está orientado a estudiantes, investigadores, ingenieros y cualquier persona que quiera experimentar, aprender o prototipar sistemas complejos de forma intuitiva, sin la barrera de la programación tradicional.

### Misión
Centralizar el flujo de trabajo experimental en una sola herramienta visual, abierta y accesible. Queremos que los usuarios se concentren en *experimentar y descubrir*, no en luchar contra la complejidad del software o los costos de las licencias.

### Visión
Un mundo donde la única barrera entre una idea experimental y su ejecución sea la curiosidad del experimentador. FloWorks aspira a ser la plataforma de referencia para la ciencia y la técnica, construida por y para la comunidad global, eliminando los muros de las herramientas privadas.

### Principios
* **Libertad Total (MIT License):** El conocimiento y las herramientas deben ser libres y accesibles para todos.
* **Extensibilidad Infinita:** Si falta un bloque, cualquiera puede crearlo e integrarlo al ecosistema usando Python.
* **Transparencia Visual:** Cada paso del proceso se puede inspeccionar, depurar y entender gráficamente.
* **Conexión con el Mundo Real:** No es solo simulación; permite controlar instrumentación científica real directamente desde el lienzo.

A diferencia de herramientas cerradas o altamente especializadas, FloWorks está diseñado como un ecosistema modular extensible donde cada componente es un nodo reutilizable y conectable.

---

## Capacidades principales

<div class="grid cards" markdown>

-   **:material-puzzle-outline: Ecosistema de Nodos Extensible**

    Catálogo técnico organizado en capas: Fuentes, Procesamiento, Control, Hardware y Scripting.

    Registro dinámico, serialización declarativa y contratos claros para desarrollo rápido.

    [:material-arrow-right: Referencia de Nodos](node-reference.md)

-   **:material-connection: Integración VISA/SCPI**

    Conexión directa con osciloscopios, LCR meters y generadores.

    Soporte multicanal, simulación integrada vía `PyVISA-py` y gestión de firewall en modo portable.

    [:material-arrow-right: Instrumentación](instrumentation.md)

-   **:material-package-variant-closed: Formato Portable `.sflow`**

    Estándar ZIP autocontenido con grafo JSON, arrays `.npy` y metadatos.

    Reproducibilidad total de experimentos y normalización DPI automática.

    [:material-arrow-right: Formato .sflow](sflow-format.md)

-   **:material-translate: Internacionalización Avanzada**

    Cambio de idioma en caliente sin reiniciar la app.

    Traducciones JSON jerárquicas y persistencia de preferencias.

    [:material-arrow-right: Guía i18n](translation-guide.md)

-   **:material-tools: SDK y Desarrollo Rápido**

    Plantilla base (`template_node.py`), mixin de serialización y guías paso a paso.

    Arquitectura preparada para plugins y expansión comunitaria.

    [:material-arrow-right: Crear Nodos](adding-a-new-node.md)

</div>

---

## Áreas de aplicación

| Área | Aplicaciones |
|------|--------------|
| 🎓 **Educación** | Física, electrónica, matemáticas, laboratorios STEM |
| ⚙️ **Ingeniería** | DSP, control, instrumentación, metrología |
| 🤖 **IA** | ML, optimización, pipelines híbridos |
| 🔬 **Investigación** | Automatización y adquisición de datos |
| 🔌 **Hardware** | VISA/SCPI, simulación y sistemas híbridos |

---

!!! tip "¿Nuevo en FloWorks?"

    Empieza por la sección **Primeros pasos con Floworks**, luego **Anatomía de la interfaz** para entender la arquitectura de la interfaz gráfica y finalmente explora **Arquitectura General** para entender el flujo de datos y la estructura del motor topológico.

---

!!! info "Modelo Open Core"

    FloWorks utiliza un modelo **Free/Open Core** bajo licencia **MIT License**.

    El núcleo permanece libre y abierto, mientras que futuras extensiones empresariales, curriculares o marketplace serán opcionales.

---

<div markdown="1" style="text-align: center;">

## FloWorks

Procesamiento visual • Instrumentación • Ciencia • IA

<small>Documentación construida con MkDocs Material</small>

</div>