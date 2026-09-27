---
title: Strumentazione VISA/SCPI
description: Guida alla connessione, configurazione e utilizzo di hardware reale e simulato in FloWorks tramite lo standard VISA/SCPI.
---

# 🔌 Strumentazione VISA/SCPI

FloWorks integra la comunicazione diretta con strumenti di laboratorio reali mediante il protocollo **SCPI** (Standard Commands for Programmable Instruments) sopra lo strato di astrazione **VISA** (Virtual Instrument Software Architecture). Inoltre, offre simulatori puramente in Python per sviluppare, testare e condividere flussi senza necessità di hardware fisico.

---

## 🌐 Cos'è VISA/SCPI?

| Tecnologia | Descrizione |
|------------|-------------|
| **VISA** | Strato standard che astrae l'interfaccia fisica (USB-TMC, Ethernet/LAN, GPIB, RS‑232). Permette di passare da uno strumento reale a uno simulato modificando solo la stringa di connessione. |
| **SCPI** | Linguaggio di comandi ASCII standardizzato per il controllo di generatori, oscilloscopi, multimetri, metri LCR, ecc. I produttori estendono lo standard, ma la base è universale. |
| **PyVISA** | Backend Python utilizzato da FloWorks. Supporta `@py` (simulazione pura) e backend nativi (`@ni`, `@ivi`, `@keysight`, ecc.). |

---

## ⚙️ Configurazione tipica

=== "📍 Stringa di connessione (Resource String)"
    Formato standard VISA:
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR` (Oscilloscopio USB)
    - `TCPIP0::192.168.1.100::inst0::INSTR` (LAN/Ethernet)
    - `ASRL1::INSTR` (Porta seriale RS-232)
    - `GPIB0::1::INSTR` (GPIB legacy)

=== "⏱️ Timeout e opzioni"
    - **Timeout**: Configurabile in ms. Aumentare se lo strumento richiede misurazioni lunghe o sweep di frequenza.
    - **Inizializzazione**: Alcuni nodi permettono di iniettare comandi SCPI personalizzati alla connessione (es. `*CLS`, `SYST:PRES`, `:CHAN1:DISP ON`).

---

## 📡 Nodi hardware disponibili

<div class="grid cards" markdown>

- **🔭 Oscilloscopio SCPI**
  Acquisisce forme d'onda nel dominio del tempo. Supporta multicanale, scalatura automatica, trigger hardware e il menu "Mostra canale" per commutare i segnali al volo.

- **⚡ Metro LCR**
  Misura impedenza, induttanza, capacità, resistenza e fattore di dissipazione. Restituisce un `master_payload` con dati primari e secondari in un'unica acquisizione.

- **🎛️ Generatore di funzioni arbitrarie**
  Invia segnali a hardware SDG o simula uscite. Configura modulazione (AM/FM/PM), sweep lineare/logaritmico, burst e fase.

- **📊 Multimetro digitale (DMM)** *(In espansione)*
  Interfaccia SCPI per misure di tensione DC/AC, corrente, resistenza e frequenza. Compatibile con Keithley, Agilent e Rigol.

- **🔋 Alimentatore programmabile** *(In espansione)*
  Controllo tensione/corrente di uscita con protezione OVP/OCP. Utile per banchi di test automatizzati.

</div>

---

## 🔄 Flusso di lavoro tipico

1. **Aggiungi il nodo** alla Tela dalla Barra degli strumenti (`Sorgenti` o `Strumenti`).
2. **Configura la connessione**: Seleziona backend, inserisci la stringa VISA e regola timeout/inizializzazione.
3. **Connetti al flusso**: Unisci l'uscita dello strumento a nodi di elaborazione (FFT, filtri, aritmetica) o visualizzazione.
4. **Esegui (`F5`)**: Il motore topologico richiede l'acquisizione, il driver analizza la risposta SCPI e impacchetta i dati.
5. **Visualizza/Esporta**: I dati scorrono nel grafo per essere elaborati dai nodi successivi.

---

## 🛠️ Risoluzione dei problemi

!!! warning "1. VISA non trova lo strumento (`VI_ERROR_RSRC_NFOUND`)"
    - **Causa:** Stringa errata, cavo scollegato o backend che non rileva il dispositivo.
    - **Soluzione:** Esegui `pyvisa-shell` o l'utilità del produttore (NI MAX, Keysight Connection Expert) per elencare le risorse valide. Verifica i permessi utente.

!!! warning "2. Timeout durante l'acquisizione"
    - **Causa:** Sweep lento, trigger non soddisfatto o strumento occupato in un'altra operazione.
    - **Soluzione:** Aumenta il timeout nel nodo. Verifica la configurazione del trigger dell'oscilloscopio (`AUTO` o `NORMAL`). Usa `*CLS` all'inizio.

!!! warning "3. La simulazione non risponde o fallisce"
    - **Causa:** `PyVISA-py` non è installato o c'è un conflitto con un altro backend.
    - **Soluzione:** `pip install pyvisa-py`. Nel nodo, seleziona esplicitamente `@py` come backend.

!!! warning "4. Errori SCPI (`Command Error`, `Execution Error`)"
    - **Causa:** Comando non supportato dal firmware o sintassi errata.
    - **Soluzione:** Consulta il manuale di programmazione SCPI del tuo strumento. Alcuni produttori richiedono prefissi `:` o terminatori `\n`. FloWorks aggiunge `\n` automaticamente, ma puoi regolare il terminatore nel driver.

!!! info "5. Creare un nodo per uno strumento non supportato"
    - Eredita da `BaseNode` e usa il pattern `DeviceBase` in `instrument/`.
    - Implementa un driver `headless` che restituisca tuple `(x, y)` o `master_payload`.
    - Segui la [📘 Guida: Aggiungere un nuovo nodo](adding-a-new-node.md) per registrare porte, serializzazione e i18n.

---

## 📚 Risorse correlate

- [🧩 Riferimento tecnico nodi](node-reference.md) → Dettagli su `oscilloscope_node`, `generator_node` e contratti di serializzazione.
- [📦 Guida build portatile](guia-ejecutable-portable.md) → Gestione firewall, `resource_path()` e pacchettizzazione PyInstaller.
- [📘 Aggiungere un nuovo nodo](adding-a-new-node.md) → Come estendere `instrument/` e registrare driver personalizzati.
- [📄 Formato `.sflow`](sflow-format.md) → Come vengono rese persistenti le configurazioni hardware e gli array acquisiti.
