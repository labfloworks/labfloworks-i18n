---
title: Instrumentace VISA/SCPI
description: Průvodce připojením, konfigurací a používáním reálného a simulovaného hardwaru ve FloWorks prostřednictvím standardu VISA/SCPI.
---

# 🔌 Instrumentace VISA/SCPI

FloWorks integruje přímou komunikaci s reálnými laboratorními přístroji prostřednictvím protokolu **SCPI** (Standard Commands for Programmable Instruments) nad vrstvou abstrakce **VISA** (Virtual Instrument Software Architecture). Kromě toho nabízí čistě Pythonové simulátory pro vývoj, testování a sdílení toků bez nutnosti fyzického hardwaru.

---

## 🌐 Co je VISA/SCPI?

| Technologie | Popis |
|-------------|-------|
| **VISA** | Standardní vrstva abstrahující fyzické rozhraní (USB-TMC, Ethernet/LAN, GPIB, RS‑232). Umožňuje přejít od reálného přístroje k simulovanému pouhou změnou připojovacího řetězce. |
| **SCPI** | Standardizovaný ASCII příkazový jazyk pro ovládání generátorů, osciloskopů, multimetrů, LCR měřičů atd. Výrobci rozšiřují standard, ale základ je univerzální. |
| **PyVISA** | Python backend používaný FloWorks. Podporuje `@py` (čistá simulace) a nativní backendy (`@ni`, `@ivi`, `@keysight`, atd.). |

---

## ⚙️ Typická konfigurace

=== "📍 Připojovací řetězec (Resource String)"
    Standardní formát VISA:
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR` (Osciloskop USB)
    - `TCPIP0::192.168.1.100::inst0::INSTR` (LAN/Ethernet)
    - `ASRL1::INSTR` (Sériový port RS-232)
    - `GPIB0::1::INSTR` (Legacy GPIB)

=== "️ Čekací doba a možnosti"
    - **Timeout**: Konfigurovatelné v ms. Zvyšte, pokud přístroj vyžaduje dlouhá měření nebo frekvenční sweepy.
    - **Inicializace**: Některé uzly umožňují vložit vlastní SCPI příkazy při připojení (např. `*CLS`, `SYST:PRES`, `:CHAN1:DISP ON`).

---

## 📡 Dostupné hardwarové uzly

<div class="grid cards" markdown>

- **🔭 SCPI Osciloskop**
  Zachytává časové průběhy. Podporuje vícekanálové měření, automatické škálování, hardwarový trigger a nabídku „Zobrazit kanál“ pro přepínání signálů za běhu.

- **⚡ LCR Meter**
  Měří impedanci, indukčnost, kapacitu, odpor a ztrátový činitel. Vrací `master_payload` s primárními a sekundárními daty v jednom snímku.

- **🎛️ Generátor libovolných funkcí**
  Odesílá signály na hardware SDG nebo simuluje výstupy. Konfiguruje modulaci (AM/FM/PM), lineární/logaritmický sweep, burst a fázi.

- **📊 Digitální multimetr (DMM)** *(Ve vývoji)*
  SCPI rozhraní pro měření stejnosměrného/střídavého napětí, proudu, odporu a frekvence. Kompatibilní s Keithley, Agilent a Rigol.

- **🔋 Programovatelný zdroj napájení** *(Ve vývoji)*
  Ovládání výstupního napětí/proudu s ochranou OVP/OCP. Užitečné pro automatizované testovací stavy.

</div>

---

## 🔄 Typický pracovní tok

1. **Přidej uzel** na Plátno z Panelu nástrojů (`Zdroje` nebo `Přístroje`).
2. **Nastav připojení**: Vyber backend, zadej VISA řetězec a uprav timeout/inicializaci.
3. **Připoj k toku**: Spoj výstup přístroje s uzly zpracování (FFT, filtry, aritmetika) nebo vizualizace.
4. **Spusť (`F5`)**: Topologický engine požádá o akvizici, driver zpracuje SCPI odpověď a zabalí data.
5. **Vizualizuj/Exportuj**: Data proudí grafem a jsou dále zpracována následujícími uzly.

---

## 🛠️ Řešení problémů

!!! warning "1. VISA nenajde přístroj (`VI_ERROR_RSRC_NFOUND`)"
    - **Příčina:** Chybný řetězec, odpojený kabel nebo backend nevidí zařízení.
    - **Řešení:** Spusť `pyvisa-shell` nebo utilitu výrobce (NI MAX, Keysight Connection Expert) pro výpis platných zdrojů. Ověř uživatelská oprávnění.

!!! warning "2. Timeout během akvizice"
    - **Příčina:** Pomalý sweep, nenaplněný trigger nebo přístroj zaneprázdněn jinou úlohou.
    - **Řešení:** Zvyš timeout v uzlu. Ověř nastavení triggeru osciloskopu (`AUTO` nebo `NORMAL`). Na začátek použij `*CLS`.

!!! warning "3. Simulace neodpovídá nebo selže"
    - **Příčina:** `PyVISA-py` není nainstalován nebo je konflikt s jiným backendem.
    - **Řešení:** `pip install pyvisa-py`. V uzlu explicitně vyber `@py` jako backend.

!!! warning "4. SCPI chyby (`Command Error`, `Execution Error`)"
    - **Příčina:** Příkaz není podporován firmwarem nebo je chybná syntaxe.
    - **Řešení:** Nahlédni do SCPI programovací příručky svého přístroje. Někteří výrobci vyžadují předpony `:` nebo ukončovače `\n`. FloWorks přidává `\n` automaticky, ale terminátor můžeš upravit v driveru.

!!! info "5. Vytvoření uzlu pro nepodporovaný přístroj"
    - Poděď se od `BaseNode` a použij vzor `DeviceBase` v `instrument/`.
    - Implementuj `headless` driver vracející dvojice `(x, y)` nebo `master_payload`.
    - Postupuj podle [📘 Průvodce: Přidat nový uzel](adding-a-new-node.md) pro registraci portů, serializaci a i18n.

---

## 📚 Související zdroje

- [🧩 Technická reference uzlů](node-reference.md) → Podrobnosti o `oscilloscope_node`, `generator_node` a serializačních kontraktech.
- [📦 Průvodce přenosným buildem](guia-ejecutable-portable.md) → Správa firewallu, `resource_path()` a balení PyInstaller.
- [📘 Přidat nový uzel](adding-a-new-node.md) → Jak rozšířit `instrument/` a registrovat vlastní drivery.
- [📄 Formát `.sflow`](sflow-format.md) → Jak se ukládají hardwarové konfigurace a zachycená pole.
