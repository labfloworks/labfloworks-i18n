---
title: FloWorks
description: Laboratorio visivo universale per l'elaborazione dei segnali, la strumentazione scientifica e l'automazione.
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## Laboratorio visivo universale per segnali, strumentazione e IA

Elaborazione scientifica • DSP • VISA/SCPI • Automazione • Machine Learning

![Schermata di FloWorks](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[Primi passi con FloWorks](getting-started.md){ .md-button }
[Anatomia dell'interfaccia](interface-anatomy.md){ .md-button .md-button--primary }
[Filosofia](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## Cos'è FloWorks?
FloWorks è un **laboratorio visivo open source** (Python + PySide6) in cui costruisci sistemi collegando blocchi (nodi) invece di scrivere righe di codice.

Immagina una Tela digitale dove unisci generatori di segnali, filtri matematici, controller hardware (VISA/SCPI) e modelli di Intelligenza Artificiale mediante cavi virtuali. Tutto si basa sul **flusso di dati**: colleghi l'uscita di un blocco con l'ingresso di un altro per elaborare informazioni, automatizzare strumenti o analizzare risultati in tempo reale.

È orientato a studenti, ricercatori, ingegneri e chiunque voglia sperimentare, imparare o realizzare prototipi di sistemi complessi in modo intuitivo, senza la barriera della programmazione tradizionale.

### Missione
Centralizzare il flusso di lavoro sperimentale in un unico strumento visivo, aperto e accessibile. Vogliamo che gli utenti si concentrino su *sperimentare e scoprire*, non a lottare contro la complessità del software o i costi delle licenze.

### Visione
Un mondo in cui l'unica barriera tra un'idea sperimentale e la sua esecuzione sia la curiosità di chi la concepisce. FloWorks aspira a essere la piattaforma di riferimento per la scienza e la tecnica, costruita dalla e per la comunità globale, eliminando i muri degli strumenti proprietari.

### Principi
* **Libertà Totale (MIT License):** La conoscenza e gli strumenti devono essere liberi e accessibili a tutti.
* **Estensibilità Infinita:** Se manca un blocco, chiunque può crearlo e integrarlo nell'ecosistema usando Python.
* **Trasparenza Visiva:** Ogni passaggio del processo può essere ispezionato, debuggato e compreso graficamente.
* **Connessione con il Mondo Reale:** Non è solo simulazione; permette di controllare strumentazione scientifica reale direttamente dalla Tela.

A differenza di strumenti chiusi o altamente specializzati, FloWorks è progettato come un ecosistema modulare ed estensibile in cui ogni componente è un nodo riutilizzabile e collegabile.

---

## Capacità principali

<div class="grid cards" markdown>

-   **:material-puzzle-outline: Ecosistema di nodi estensibile**

    Catalogo tecnico organizzato in strati: Sorgenti, Elaborazione, Controllo, Hardware e Scripting.

    Registro dinamico, serializzazione dichiarativa e contratti chiari per uno sviluppo rapido.

    [:material-arrow-right: Riferimento nodi](node-reference.md)

-   **:material-connection: Integrazione VISA/SCPI**

    Connessione diretta con oscilloscopi, meter LCR e generatori.

    Supporto multicanale, simulazione integrata via `PyVISA-py` e gestione firewall in modalità portatile.

    [:material-arrow-right: Strumentazione](instrumentation.md)

-   **:material-package-variant-closed: Formato portatile `.sflow`**

    Standard ZIP autocontenuto con grafo JSON, array `.npy` e metadati.

    Reproducibilità totale degli esperimenti e normalizzazione DPI automatica.

    [:material-arrow-right: Formato .sflow](sflow-format.md)

-   **:material-translate: Internazionalizzazione avanzata**

    Cambio di lingua al volo senza riavviare l'app.

    Traduzioni JSON gerarchiche e persistenza delle preferenze.

    [:material-arrow-right: Guida i18n](translation-guide.md)

-   **:material-tools: SDK e sviluppo rapido**

    Template base (`template_node.py`), mixin di serializzazione e guide passo passo.

    Architettura predisposta per plugin ed espansione comunitaria.

    [:material-arrow-right: Creare nodi](adding-a-new-node.md)

</div>

---

## Aree di applicazione

| Area | Applicazioni |
|------|--------------|
| 🎓 **Educazione** | Fisica, elettronica, matematica, laboratori STEM |
| ⚙️ **Ingegneria** | DSP, controllo, strumentazione, metrologia |
| 🤖 **IA** | ML, ottimizzazione, pipeline ibride |
| 🔬 **Ricerca** | Automazione e acquisizione dati |
| 🔌 **Hardware** | VISA/SCPI, simulazione e sistemi ibridi |

---

!!! tip "Nuovo su FloWorks?"

    Inizia dalla sezione **Primi passi con FloWorks**, poi leggi **Anatomia dell'interfaccia** per capire l'architettura dell'interfaccia grafica ed esplora infine **Architettura Generale** per comprendere il flusso di dati e la struttura del motore topologico.

---

!!! info "Modello Open Core"

    FloWorks utilizza un modello **Free/Open Core** sotto licenza **MIT License**.

    Il nucleo resta libero e aperto, mentre future estensioni enterprise, curricolari o marketplace saranno opzionali.

---

<div markdown="1" style="text-align: center;">

## FloWorks

Elaborazione visiva • Strumentazione • Scienza • IA

<small>Documentazione costruita con MkDocs Material</small>

</div>
