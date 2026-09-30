---
title: FloWorks
description: Universal visual laboratory for signal processing, scientific instrumentation and automation.
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## Universal visual laboratory for signals, instrumentation and AI

Scientific processing • DSP • VISA/SCPI • Automation • Machine Learning

![FloWorks screenshot](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[Getting Started with FloWorks](getting-started.md){ .md-button }
[Anatomy of the Main Interface](interface-anatomy.md){ .md-button .md-button--primary }
[Philosophy](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## What is FloWorks?
FloWorks is an **open-source visual laboratory** (Python + PySide6) where you build systems by connecting blocks (nodes) instead of writing lines of code.

Imagine a digital canvas where you link signal generators, mathematical filters, hardware controllers (VISA/SCPI) and Artificial Intelligence models through virtual wires. Everything is based on **data flow**: you connect the output of one block to the input of another to process information, automate equipment or analyze results in real time.

It is aimed at students, researchers, engineers and anyone who wants to experiment, learn or prototype complex systems intuitively, without the barrier of traditional programming.

### Mission
To centralize the experimental workflow in a single visual, open and accessible tool. We want users to focus on *experimenting and discovering*, not on struggling against software complexity or license costs.

### Vision
A world where the only barrier between an experimental idea and its execution is the curiosity of the experimenter. FloWorks aspires to be the reference platform for science and engineering, built by and for the global community, tearing down the walls of proprietary tools.

### Principles
* **Total Freedom:** Knowledge and tools must be accessible to everyone. FloWorks is free to use and committed to an open, extensible core.
* **Infinite Extensibility:** If a block is missing, anyone can create it and integrate it into the ecosystem using Python.
* **Visual Transparency:** Every step of the process can be inspected, debugged and understood graphically.
* **Connection with the Real World:** It is not just simulation; it allows controlling real scientific instrumentation directly from the canvas.

Unlike closed or highly specialized tools, FloWorks is designed as an extensible modular ecosystem where every component is a reusable, connectable node.

---

## Main capabilities

<div class="grid cards" markdown>

-   **:material-puzzle-outline: Extensible Node Ecosystem**

    Technical catalog organized in layers: Sources, Processing, Control, Hardware and Scripting.

    Dynamic registry, declarative serialization and clear contracts for rapid development.

    [:material-arrow-right: Node Reference](node-reference.md)

-   **:material-connection: VISA/SCPI Integration**

    Direct connection with oscilloscopes, LCR meters and generators.

    Multichannel support, integrated simulation via `PyVISA-py` and firewall management in portable mode.

    [:material-arrow-right: Instrumentation](instrumentation.md)

-   **:material-package-variant-closed: Portable `.sflow` Format**

    Self-contained ZIP standard with JSON graph, `.npy` arrays and metadata.

    Full reproducibility of experiments and automatic DPI normalization.

    [:material-arrow-right: .sflow Format](sflow-format.md)

-   **:material-translate: Advanced Internationalization**

    Hot language switching without restarting the app.

    Hierarchical JSON translations and preference persistence.

    [:material-arrow-right: i18n Guide](translation-guide.md)

-   **:material-tools: SDK and Rapid Development**

    Base template (`template_node.py`), serialization mixin and step-by-step guides.

    Architecture ready for plugins and community expansion.

    [:material-arrow-right: Create Nodes](adding-a-new-node.md)

</div>

---

## Application areas

| Area | Applications |
|------|--------------|
| 🎓 **Education** | Physics, electronics, mathematics, STEM laboratories |
| ⚙️ **Engineering** | DSP, control, instrumentation, metrology |
| 🤖 **AI** | ML, optimization, hybrid pipelines |
| 🔬 **Research** | Automation and data acquisition |
| 🔌 **Hardware** | VISA/SCPI, simulation and hybrid systems |

---

!!! tip "New to FloWorks?"

    Start with the **Getting Started with FloWorks** section, then **Anatomy of the Interface** to understand the GUI architecture and finally explore **General Architecture** to understand the data flow and the topological engine structure.

---

<div markdown="1" style="text-align: center;">

## FloWorks

Visual Processing • Instrumentation • Science • AI

<small>Documentation built with MkDocs Material</small>

</div>
