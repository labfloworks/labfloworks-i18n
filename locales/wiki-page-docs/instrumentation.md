---
title: VISA/SCPI Instrumentation
description: Guide for connecting, configuring and using real and simulated hardware in FloWorks via the VISA/SCPI standard.
---

# 🔌 VISA/SCPI Instrumentation

FloWorks integrates direct communication with real laboratory instruments via the **SCPI** (Standard Commands for Programmable Instruments) protocol over the **VISA** (Virtual Instrument Software Architecture) abstraction layer. It also offers purely Python-based simulators to develop, test and share flows without needing physical hardware.

---

## 🌐 What is VISA/SCPI?

| Technology | Description |
|------------|-------------|
| **VISA** | Standard layer that abstracts the physical interface (USB-TMC, Ethernet/LAN, GPIB, RS‑232). Allows switching from a real instrument to a simulated one just by modifying the connection string. |
| **SCPI** | Standardized ASCII command language for controlling generators, oscilloscopes, multimeters, LCR meters, etc. Manufacturers extend the standard, but the base is universal. |
| **PyVISA** | Python backend used by FloWorks. Supports `@py` (pure simulation) and native backends (`@ni`, `@ivi`, `@keysight`, etc.). |

---

## ⚙️ Typical Configuration

=== "📍 Connection String (Resource String)"
    Standard VISA format:
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR` (USB oscilloscope)
    - `TCPIP0::192.168.1.100::inst0::INSTR` (LAN/Ethernet)
    - `ASRL1::INSTR` (RS-232 serial port)
    - `GPIB0::1::INSTR` (legacy GPIB)

=== "⏱️ Timeout and Options**"
    - **Timeout**: Configurable in ms. Increase if the instrument requires long measurements or frequency sweeps.
    - **Initialization**: Some nodes allow injecting custom SCPI commands on connect (e.g. `*CLS`, `SYST:PRES`, `:CHAN1:DISP ON`).

---

## 📡 Available Hardware Nodes

<div class="grid cards" markdown>

- **🔭 SCPI Oscilloscope**
  Captures time-domain waveforms. Supports multichannel, auto-scaling, hardware trigger and a "Show channel" menu to toggle signals on the fly.

- **⚡ LCR Meter**
  Measures impedance, inductance, capacitance, resistance and dissipation factor. Returns a `master_payload` with primary and secondary data in a single acquisition.

- **🎛️ Arbitrary Function Generator**
  Sends signals to SDG hardware or simulates outputs. Configures modulation (AM/FM/PM), linear/logarithmic sweep, burst and phase.

- **📊 Digital Multimeter (DMM)** *(In expansion)*
  SCPI interface for DC/AC voltage, current, resistance and frequency measurements. Compatible with Keithley, Agilent and Rigol.

- **🔋 Programmable Power Supply** *(In expansion)*
  Output voltage/current control with OVP/OCP protection. Useful for automated test benches.

</div>

---

## 🔄 Typical Workflow

1. **Add the node** to the canvas from the toolbar (`Sources` or `Instruments`).
2. **Configure the connection**: Select backend, enter the VISA string and adjust timeout/initialization.
3. **Connect to the flow**: Link the instrument output to processing nodes (FFT, filters, arithmetic) or visualization.
4. **Run (`F5`)**: The topological engine requests acquisition, the driver parses the SCPI response and packages the data.
5. **Visualize/Export**: Data flows through the graph to be processed by subsequent nodes.

---

## 🛠️ Troubleshooting

!!! warning "1. VISA cannot find the instrument (`VI_ERROR_RSRC_NFOUND`)"
    - **Cause:** Incorrect string, disconnected cable or backend does not detect the device.
    - **Solution:** Run `pyvisa-shell` or the manufacturer's utility (NI MAX, Keysight Connection Expert) to list valid resources. Verify user permissions.

!!! warning "2. Timeout during acquisition"
    - **Cause:** Slow sweep, trigger not met or instrument busy with another task.
    - **Solution:** Increase the timeout in the node. Verify that the oscilloscope trigger is configured correctly (`AUTO` or `NORMAL`). Use `*CLS` at startup.

!!! warning "3. Simulation does not respond or fails"
    - **Cause:** `PyVISA-py` is not installed or there is a conflict with another backend.
    - **Solution:** `pip install pyvisa-py`. In the node, explicitly select `@py` as the backend.

!!! warning "4. SCPI errors (`Command Error`, `Execution Error`)"
    - **Cause:** Command not supported by the firmware or incorrect syntax.
    - **Solution:** Consult your instrument's SCPI programming manual. Some manufacturers require `:` prefixes or `
` terminators. FloWorks adds `
` automatically, but you can adjust the terminator in the driver.

!!! info "5. Creating a node for an unsupported instrument"
    - Inherit from `BaseNode` and use the `DeviceBase` pattern in `instrument/`.
    - Implement a `headless` driver that returns `(x, y)` tuples or `master_payload`.
    - Follow the [📘 Guide: Add a New Node](adding-a-new-node.md) to register ports, serialization and i18n.

---

## 📚 Related Resources

- [🧩 Technical Node Reference](node-reference.md) → Details of `oscilloscope_node`, `generator_node` and serialization contracts.
- [📦 Portable Build Guide](guia-ejecutable-portable.md) → Firewall handling, `resource_path()` and PyInstaller packaging.
- [📘 Add a New Node](adding-a-new-node.md) → How to extend `instrument/` and register custom drivers.
- [📄 `.sflow` Format](sflow-format.md) → How hardware configurations and captured arrays are persisted.
