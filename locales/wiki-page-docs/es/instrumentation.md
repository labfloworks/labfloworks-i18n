---
title: Instrumentación VISA/SCPI
description: Guía de conexión, configuración y uso de hardware real y simulado en FloWorks mediante el estándar VISA/SCPI.
---

# 🔌 Instrumentación VISA/SCPI

FloWorks integra comunicación directa con instrumentos de laboratorio reales mediante el protocolo **SCPI** (Standard Commands for Programmable Instruments) sobre la capa de abstracción **VISA** (Virtual Instrument Software Architecture). Además, ofrece simuladores puramente en Python para desarrollar, probar y compartir flujos sin necesidad de hardware físico.

---

## 🌐 ¿Qué es VISA/SCPI?

| Tecnología | Descripción |
|------------|-------------|
| **VISA** | Capa estándar que abstrae la interfaz física (USB-TMC, Ethernet/LAN, GPIB, RS‑232). Permite cambiar de un instrumento real a uno simulado solo modificando la cadena de conexión. |
| **SCPI** | Lenguaje de comandos ASCII estandarizado para controlar generadores, osciloscopios, multímetros, LCR meters, etc. Los fabricantes extienden el estándar, pero la base es universal. |
| **PyVISA** | Backend Python utilizado por FloWorks. Soporta `@py` (simulación pura) y backends nativos (`@ni`, `@ivi`, `@keysight`, etc.). |

---

## ⚙️ Configuración Típica

=== "📍 Cadena de Conexión (Resource String)"
    Formato estándar VISA:
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR` (Osciloscopio USB)
    - `TCPIP0::192.168.1.100::inst0::INSTR` (LAN/Ethernet)
    - `ASRL1::INSTR` (Puerto serial RS-232)
    - `GPIB0::1::INSTR` (GPIB legacy)

=== "⏱️ Tiempo de Espera y Opciones**"
    - **Timeout**: Configurable en ms. Aumenta si el instrumento requiere mediciones largas o barridos de frecuencia.
    - **Inicialización**: Algunos nodos permiten inyectar comandos SCPI personalizados al conectar (ej. `*CLS`, `SYST:PRES`, `:CHAN1:DISP ON`).

---

## 📡 Nodos de Hardware Disponibles

<div class="grid cards" markdown>

- **🔭 Osciloscopio SCPI**  
  Captura formas de onda en dominio temporal. Soporta multicanal, escalado automático, trigger hardware y menú "Mostrar canal" para alternar señales en caliente.

- **⚡ LCR Meter**  
  Mide impedancia, inductancia, capacitancia, resistencia y factor de pérdida. Retorna un `master_payload` con datos primarios y secundarios en una sola adquisición.

- **🎛️ Generador de Funciones Arbitrarias**  
  Envía señales a hardware SDG o simula salidas. Configura modulación (AM/FM/PM), sweep lineal/logarítmico, burst y fase.

- **📊 Multímetro Digital (DMM)** *(En expansión)*  
  Interfaz SCPI para medidas de tensión DC/AC, corriente, resistencia y frecuencia. Compatible con Keithley, Agilent y Rigol.

- **🔋 Fuente de Alimentación Programable** *(En expansión)*  
  Control de tensión/corriente de salida con protección OVP/OCP. Útil para bancos de pruebas automatizados.

</div>

---

## 🔄 Flujo de Trabajo Típico

1. **Añadir el nodo** al lienzo desde toolbar (`Fuentes` o `Instrumentos`).
2. **Configurar la conexión**: Selecciona backend, introduce la cadena VISA y ajusta timeout/inicialización.
3. **Conectar al flujo**: Une la salida del instrumento a nodos de procesamiento (FFT, filtros, aritmética) o visualización.
4. **Ejecutar (`F5`)**: El motor topológico solicita la adquisición, el driver parsea la respuesta SCPI y empaqueta los datos.
5. **Visualizar/Exportar**: Los datos fluyen por el grafo para ser procesados por los siguientes nodos. 

---

## 🛠️ Solución de Problemas

!!! warning "1. VISA no encuentra el instrumento (`VI_ERROR_RSRC_NFOUND`)"
    - **Causa:** Cadena incorrecta, cable desconectado o backend no detecta el dispositivo.
    - **Solución:** Ejecuta `pyvisa-shell` o la utilidad del fabricante (NI MAX, Keysight Connection Expert) para listar recursos válidos. Verifica permisos de usuario.

!!! warning "2. Timeout durante la adquisición"
    - **Causa:** Barrido lento, trigger no se cumple o instrumento ocupado en otra tarea.
    - **Solución:** Aumenta el timeout en el nodo. Verifica que el trigger del osciloscopio esté configurado correctamente (`AUTO` o `NORMAL`). Usa `*CLS` al inicio.

!!! warning "3. Simulación no responde o falla"
    - **Causa:** `PyVISA-py` no está instalado o hay conflicto con otro backend.
    - **Solución:** `pip install pyvisa-py`. En el nodo, selecciona explícitamente `@py` como backend.

!!! warning "4. Errores SCPI (`Command Error`, `Execution Error`)"
    - **Causa:** Comando no soportado por el firmware o sintaxis incorrecta.
    - **Solución:** Consulta el manual de programación SCPI de tu instrumento. Algunos fabricantes requieren prefijos `:` o terminadores `\n`. FloWorks añade `\n` automáticamente, pero puedes ajustar el terminador en el driver.

!!! info "5. Crear un nodo para un instrumento no soportado"
    - Hereda de `BaseNode` y usa el patrón `DeviceBase` en `instrument/`.
    - Implementa un driver `headless` que retorne tuplas `(x, y)` o `master_payload`.
    - Sigue la [📘 Guía: Añadir un Nuevo Nodo](adding-a-new-node.md) para registrar puertos, serialización y i18n.

---

## 📚 Recursos Relacionados

- [🧩 Referencia Técnica de Nodos](node-reference.md) → Detalles de `oscilloscope_node`, `generator_node` y contratos de serialización.
- [📦 Guía de Build Portable](guia-ejecutable-portable.md) → Manejo de firewall, `resource_path()` y empaquetado PyInstaller.
- [📘 Añadir un Nuevo Nodo](adding-a-new-node.md) → Cómo extender `instrument/` y registrar drivers personalizados.
- [📄 Formato `.sflow`](sflow-format.md) → Cómo se persisten las configuraciones de hardware y los arrays capturados.