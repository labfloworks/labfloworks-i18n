---
title: Instrumentație VISA/SCPI
description: Ghid de conectare, configurare și utilizare a hardware-ului real și simulat în FloWorks prin standardul VISA/SCPI.
---

# 🔌 Instrumentație VISA/SCPI

FloWorks integrează comunicarea directă cu instrumente de laborator reale prin protocolul **SCPI** (Standard Commands for Programmable Instruments) peste stratul de abstractizare **VISA** (Virtual Instrument Software Architecture). De asemenea, oferă simulatoare pur în Python pentru dezvoltarea, testarea și partajarea fluxurilor fără a necesita hardware fizic.

---

## 🌐 Ce este VISA/SCPI?

| Tehnologie | Descriere |
|------------|-----------|
| **VISA** | Strat standard care abstractizează interfața fizică (USB-TMC, Ethernet/LAN, GPIB, RS‑232). Permite trecerea de la un instrument real la unul simulat doar prin modificarea șirului de conexiune. |
| **SCPI** | Limbaj de comenzi ASCII standardizat pentru controlul generatoarelor, osciloscoapelor, multimetrelor, metrelor LCR etc. Producătorii extind standardul, dar baza este universală. |
| **PyVISA** | Backend Python utilizat de FloWorks. Suportă `@py` (simulare pură) și backend-uri native (`@ni`, `@ivi`, `@keysight`, etc.). |

---

## ⚙️ Configurație tipică

=== "📍 Șir de conexiune (Resource String)"
    Format standard VISA:
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR` (Osciloscop USB)
    - `TCPIP0::192.168.1.100::inst0::INSTR` (LAN/Ethernet)
    - `ASRL1::INSTR` (Port serial RS-232)
    - `GPIB0::1::INSTR` (GPIB legacy)

=== "⏱️ Timp de așteptare și opțiuni"
    - **Timeout**: Configurabil în ms. Mărește dacă instrumentul necesită măsurători lungi sau sweep-uri de frecvență.
    - **Inițializare**: Unele noduri permit injectarea de comenzi SCPI personalizate la conectare (ex. `*CLS`, `SYST:PRES`, `:CHAN1:DISP ON`).

---

## 📡 Noduri hardware disponibile

<div class="grid cards" markdown>

- **🔭 Osciloscop SCPI**
  Capturează forme de undă în domeniul timp. Suportă multicanal, scalare automată, trigger hardware și meniul „Afișează canal" pentru comutarea semnalelor în timp real.

- **⚡ Metru LCR**
  Măsoară impedanța, inductanța, capacitatea, rezistența și factorul de pierdere. Returnează un `master_payload` cu date primare și secundare într-o singură achiziție.

- **🎛️ Generator de funcții arbitrare**
  Trimite semnale către hardware SDG sau simulează ieșiri. Configurează modulația (AM/FM/PM), sweep liniar/logaritmic, burst și fază.

- **📊 Multimetru digital (DMM)** *(În extindere)*
  Interfață SCPI pentru măsurători de tensiune DC/AC, curent, rezistență și frecvență. Compatibil cu Keithley, Agilent și Rigol.

- **🔋 Sursă de alimentare programabilă** *(În extindere)*
  Control al tensiunii/curentului de ieșire cu protecție OVP/OCP. Util pentru bancuri de testare automatizate.

</div>

---

## 🔄 Flux de lucru tipic

1. **Adaugă nodul** pe Pânză din Bară de unelte (`Surse` sau `Instrumente`).
2. **Configurează conexiunea**: Selectează backend, introdu șirul VISA și ajustează timeout/inițializarea.
3. **Conectează la flux**: Unește ieșirea instrumentului cu noduri de procesare (FFT, filtre, aritmetică) sau vizualizare.
4. **Rulează (`F5`)**: Motorul topologic solicită achiziția, driverul parsează răspunsul SCPI și împachetează datele.
5. **Vizualizează/Exportă**: Datele curg prin graf pentru a fi prelucrate de nodurile următoare.

---

## 🛠️ Depanare

!!! warning "1. VISA nu găsește instrumentul (`VI_ERROR_RSRC_NFOUND`)"
    - **Cauză:** Șir incorect, cablu deconectat sau backend-ul nu detectează dispozitivul.
    - **Soluție:** Rulează `pyvisa-shell` sau utilitarul producătorului (NI MAX, Keysight Connection Expert) pentru a lista resurse valide. Verifică permisiunile utilizatorului.

!!! warning "2. Timeout în timpul achiziției"
    - **Cauză:** Sweep lent, triggerul nu este îndeplinit sau instrumentul este ocupat cu altă sarcină.
    - **Soluție:** Mărește timeout-ul în nod. Verifică configurarea triggerului osciloscopului (`AUTO` sau `NORMAL`). Folosește `*CLS` la început.

!!! warning "3. Simularea nu răspunde sau eșuează"
    - **Cauză:** `PyVISA-py` nu este instalat sau există un conflict cu alt backend.
    - **Soluție:** `pip install pyvisa-py`. În nod, selectează explicit `@py` ca backend.

!!! warning "4. Erori SCPI (`Command Error`, `Execution Error`)"
    - **Cauză:** Comandă neacceptată de firmware sau sintaxă incorectă.
    - **Soluție:** Consultă manualul de programare SCPI al instrumentului. Unii producători necesită prefixe `:` sau terminatori `\n`. FloWorks adaugă `\n` automat, dar poți ajusta terminatorul în driver.

!!! info "5. Crearea unui nod pentru un instrument neacceptat"
    - Moștenește din `BaseNode` și folosește pattern-ul `DeviceBase` în `instrument/`.
    - Implementează un driver `headless` care returnează tupluri `(x, y)` sau `master_payload`.
    - Urmează [📘 Ghid: Adaugă un nod nou](adding-a-new-node.md) pentru înregistrarea porturilor, serializării și i18n.

---

## 📚 Resurse conexe

- [🧩 Referință tehnică noduri](node-reference.md) → Detalii despre `oscilloscope_node`, `generator_node` și contractele de serializare.
- [📦 Ghid build portabil](guia-ejecutable-portable.md) → Gestionare firewall, `resource_path()` și ambalare PyInstaller.
- [📘 Adaugă un nod nou](adding-a-new-node.md) → Cum extinzi `instrument/` și înregistrezi drivere personalizate.
- [📄 Format `.sflow`](sflow-format.md) → Cum se persistă configurațiile hardware și array-urile capturate.
