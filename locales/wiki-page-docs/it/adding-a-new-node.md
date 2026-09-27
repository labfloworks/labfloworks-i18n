---
title: Guida per Aggiungere un Nuovo Nodo a FloWorks
description: Tutorial passo passo per creare, registrare e integrare nodi personalizzati nel motore di flusso di FloWorks.
---

# 📘 Guida per Sviluppatori: Come Aggiungere un Nuovo Nodo a FloWorks

Questa guida descrive il processo completo per creare un nuovo tipo di nodo in FloWorks, assicurando che si integri correttamente con il motore di flusso, l'interfaccia utente, i temi visivi e il sistema di internazionalizzazione.

---

## 📋 Indice
- [📘 Guida per Sviluppatori: Come Aggiungere un Nuovo Nodo a FloWorks](#-guida-per-sviluppatori-come-aggiungere-un-nuovo-nodo-a-floworks)
  - [📋 Indice](#-indice)
  - [1. Introduzione all'Architettura](#1-introduzione-allarchitettura)
  - [2. Utilizzo del Template `template_node.py`](#2-utilizzo-del-template-template_nodepy)
  - [3. Passo dopo Passo: Creazione di un Nodo Personalizzato](#3-passo-dopo-passo-creazione-di-un-nodo-personalizzato)
    - [3.1. Copiare e Rinominare il Template](#31-copiare-e-rinominare-il-template)
    - [3.2. Definire Porte ed Etichette](#32-definire-porte-ed-etichette)
    - [3.3. Implementare la Logica di Elaborazione](#33-implementare-la-logica-di-elaborazione)
    - [3.4. Personalizzare l'Aspetto (Opzionale)](#34-personalizzare-laspetto-opzionale)
    - [3.5. Aggiungere Parametri Configurabili (Opzionale)](#35-aggiungere-parametri-configurabili-opzionale)
    - [3.6. Rendere il Nodo Serializzabile (salvare / caricare configurazioni)](#36-rendere-il-nodo-serializzabile-salvare--caricare-configurazioni)
  - [4. Integrazione nel Sistema](#4-integrazione-nel-sistema)
  - [5. Internazionalizzazione (i18n)](#5-internazionalizzazione-i18n)
  - [6. Temi Visivi](#6-temi-visivi)
  - [7. Lista di Controllo e Risoluzione dei Problemi](#7-lista-di-controllo-e-risoluzione-dei-problemi)
    - [✅ Lista di Controllo](#-lista-di-controllo)
    - [🐛 Problemi Comuni](#-problemi-comuni)
  - [8. Conclusione](#8-conclusione)

---

## 1. Introduzione all'Architettura

FloWorks e costruito su PySide6 e utilizza un modello di nodi connettibili che rappresentano un flusso di elaborazione dei segnali.

---

## 2. Utilizzo del Template `template_node.py`

Per facilitare la creazione di nuovi nodi, viene fornito il file `nodes/template_node.py`. Questo template include:
- Supporto completo per l'internazionalizzazione (connessione a `languageChanged`, metodo `update_language`).
- Supporto completo per i temi (metodo `update_theme`).
- Aiuto integrato in formato HTML a tre sezioni.
- Gestione di multiple porte di ingresso/uscita configurabili.
- Uscite multiple con `get_output_for_port`.
- Visualizzazione nel plot tramite `get_display_signal`.
- Menu contestuale traducibile.

Si consiglia di partire sempre da questo template quando si sviluppa un nuovo nodo.

---

## 3. Passo dopo Passo: Creazione di un Nodo Personalizzato

### 3.1. Copiare e Rinominare il Template
1. Copia `nodes/template_node.py` con il nome del tuo nuovo nodo, ad esempio `nodes/mi_nodo.py`.
2. Rinomina la classe da `TemplateNode` a qualcosa di descrittivo, es. `MiNodoNode`.
3. Aggiusta gli import se necessario.

### 3.2. Definire Porte ed Etichette
!!! warning "Importante: Coincidenza dei Nomi"
    I nomi delle porte in `PORTS`, `PORT_LABELS` e le chiavi del dizionario restituito da `execute_program` devono essere **esattamente uguali** (incluse maiuscole/minuscole). Il template ora include una mappatura degli alias (`'data_in'` -> prima porta sinistra) per maggiore robustezza.

Modifica il dizionario `PORTS` nella parte superiore del file. Ogni voce ha il formato:
```python
"nome_porta": ("lato", frazione)
```
- **Lati possibili:** `"left"`, `"right"`, `"top"`, `"bottom"`.
- **Frazione:** valore tra `0.0` e `1.0` che indica la posizione lungo il lato.

**Esempio per un nodo con un ingresso e due uscite:**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
Il dizionario `PORT_LABELS` contiene il testo che apparira accanto a ogni porta. Si consiglia di usare chiavi di traduzione invece di testo fisso (vedi sezione Internazionalizzazione).

### 3.3. Implementare la Logica di Elaborazione
Il metodo chiave e `execute_program(self, input_data)`. Questo metodo viene invocato dal motore di flusso quando il nodo riceve dati.

**`input_data` puo essere:**
- `None` se non c'e ingresso.
- Una tupla `(x, y)` per segnali temporali.
- Un array 1D.
- Un dizionario `{nome_porta: dati}` in nodi con ingressi multipli.

**Valore di ritorno:**
- Per nodi con un'unica uscita, restituire direttamente i dati (es. tupla `(x, y)`).
- Per nodi con uscite multiple, restituire un dizionario dove le chiavi coincidono con i nomi delle porte di uscita definite in `PORTS`.

```python
def execute_program(self, input_data):
    # Elabora input_data e genera risultati
    risultato_magnitude = (freq, mag)
    risultato_phase = (freq, phase)
    return {
        "magnitude": risultato_magnitude,
        "phase": risultato_phase
    }
```

!!! tip "Nota sui nomi porta generici"
    Il motore di flusso puo occasionalmente passare un dizionario con chiavi come `'data_in'` invece del nome reale della porta (specialmente se l'utente non ha fatto clic esattamente sul cerchio). Il template include gia codice per gestire questo caso:
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    Questo evita che il nodo fallisca per un errore di connessione impreciso.

Il template include gia un esempio commentato. Inoltre, implementa `get_output_for_port(self, port_name)` affinche il motore possa instradare ogni uscita:
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. Personalizzare l'Aspetto (Opzionale)
Il metodo `paint()` disegna lo sfondo, il titolo, lo stato e qualsiasi testo aggiuntivo. Puoi modificare:
- I colori (si aggiornano automaticamente con `update_theme`).
- Il testo di stato (usando l'attributo `self._status`).
- Informazioni di riepilogo (es. picco di magnitudine).

Il template mostra un esempio di base.

### 3.5. Aggiungere Parametri Configurabili (Opzionale)
Se il tuo nodo richiede parametri regolabili dall'utente (es. dimensione finestra, frequenza di taglio), puoi:
1. Aggiungere attributi in `__init__` (es. `self.window_size = 512`).
2. Creare una finestra di configurazione (eredita da `QDialog`).
3. Connettere la finestra in `open_config_dialog()` (metodo gia presente nel template).
4. Aggiornare i parametri dalla finestra e chiamare `self.update()`.

### 3.6. Rendere il Nodo Serializzabile (salvare / caricare configurazioni)
Affinche il nodo possa salvare e recuperare i propri parametri durante copia/incolla, annulla/ripeti, o usando i comandi Salva/Apri del menu File, deve ereditare dal mixin di serializzazione e dichiarare i propri attributi.

1. Importa il mixin nel tuo file:
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. Cambia l'ereditarieta della classe per includerlo prima di `QGraphicsObject`:
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
    ```
3. Definisci la lista `SERIALISABLE` a livello di classe, con i nomi degli attributi che vuoi persistere. Accetta solo tipi semplici (`int`, `float`, `str`, `bool`), liste, dizionari, o array NumPy (questi ultimi vengono memorizzati automaticamente come file `.npy` all'interno di `.sflow`).
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frecuencia', 'amplitud', 'configuracion']
    ```
4. Assicurati che quegli attributi siano inizializzati in `__init__`:
    ```python
    self.frecuencia = 1000.0
    self.amplitud = 1.0
    self.configuracion = {'tipo': 'seno', 'fase': 0}
    ```

Con questo, non e necessario scrivere metodi `serialize`/`deserialize`; il mixin si occupa automaticamente di salvare e recuperare i valori.

Se il tuo nodo richiede logica aggiuntiva al caricamento (per esempio, riconnettere uno strumento hardware), puoi sovrascrivere `deserialize` chiamando prima il metodo padre:
```python
def deserialize(self, data):
    super().deserialize(data)   # ripristina gli attributi da SERIALISABLE
    self._iniciar_dispositivo()
```

---

## 4. Integrazione nel Sistema

Una volta creato il file del nodo, basta incollarlo nella cartella `nodes` affinche appaia nell'interfaccia e funzioni con il resto del sistema.

---

## 5. Internazionalizzazione (i18n)

Tutti i testi visibili devono essere traducibili tramite `tr("chiave", default="...")`. Il template lo implementa gia. Devi aggiungere le chiavi corrispondenti nei file JSON all'interno di `locales/`.

**Struttura consigliata:**
```json
{
   "nodes": {
     "mi_nodo": {
       "title": "Mi Nodo",
       "tooltip": "Descrizione popup",
       "ports": {
         "input": "Ingresso",
         "output1": "Uscita 1",
         "output2": "Uscita 2"
      },
       "status": {
         "no_data": "Nessun dato",
         "ready": "Pronto"
      },
       "menu": {
         "show_output": "Mostra uscita",
         "configure": "Configura..."
      },
       "help_title": "Aiuto - Mi Nodo",
       "help_html": "<h3>🎛️ Filter Node</h3>
<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_mi_nodo": "Mi Nodo"
  }
}
```

L'aiuto HTML segue il formato a tre sezioni comune a tutti i nodi (descrizione specifica + "Come pensare al sistema" + "Scorciatoie e trucchi"). Il template include gia la struttura in `get_help_text()`.

---

## 6. Temi Visivi

Il metodo `update_theme(self, theme)` riceve un dizionario con i colori definiti dal tema attuale. Il template aggiorna automaticamente:
- Sfondo del nodo (`node_normal_bg`)
- Bordo (`node_selected_border`)
- Colore del titolo e del testo (`node_normal_text`)
- Colori delle porte (`port_circle`, `port_outline`, `port_inline`, `port_text`)

Assicurati che in `MainWindow` (o `ThemeUpdater`) venga chiamato `node.update_theme()` per ogni nodo quando il tema cambia.

---

## 7. Lista di Controllo e Risoluzione dei Problemi

### ✅ Lista di Controllo
- [ ] Il nodo si crea correttamente dalla barra degli strumenti.
- [ ] Le porte si mostrano nelle posizioni previste e sono rilevabili per le connessioni (`Ctrl+clic`).
- [ ] Alla ricezione dei dati di ingresso, viene chiamato `execute_program` e il segnale viene elaborato.
- [ ] Le uscite si propagano correttamente ai nodi connessi.
- [ ] Il menu contestuale permette di cambiare il canale di visualizzazione (se ci sono uscite multiple).
- [ ] Cliccando sul nodo, il segnale selezionato viene disegnato nel widget plot.
- [ ] Il doppio clic apre l'aiuto nel formato adeguato.
- [ ] La lingua cambia correttamente (testi di titoli, porte, menu).
- [ ] Il tema cambia correttamente (colori del nodo e delle porte).
- [ ] Copia/incolla funziona senza errori.

!!! tip "Connessione precisa delle porte"
    Quando colleghi i nodi, assicurati di fare clic esattamente sul cerchio della porta di destinazione. Se fai clic sul corpo del nodo, il sistema usera un nome generico (`'data_in'`). Il template ora tollera questi nomi, ma e una buona pratica collegarsi direttamente al cerchio per garantire il corretto instradamento delle uscite multiple.

### 🐛 Problemi Comuni

| Sintomo | Possibile Causa | Soluzione |
|---------|---------------|----------|
| La freccia di connessione non si ancora alla porta. | Il cerchio della porta non ha `setData(0, port_name)` oppure `get_port_scene_pos` non e implementato. | Verificare che in `_create_ports` venga fatto `circle.setData(0, port_name)` e che `get_port_scene_pos` usi quel nome. |
| Le uscite non arrivano ai nodi connessi. | `execute_program` non restituisce un dizionario (per uscite multiple) oppure `get_output_for_port` non e implementato. | Assicurarsi che `execute_program` restituisca `{nome_porta: dati}` e che `get_output_for_port` restituisca il valore corrispondente. |
| Cliccando sul nodo non si disegna nulla. | `get_display_signal` non restituisce una tupla `(x, y)` valida oppure `display_channel` non coincide con un'uscita esistente. | Controllare che `get_display_signal` usi il canale selezionato e che i dati siano array NumPy. |
| I testi non si aggiornano al cambio di lingua. | Non e stato collegato il segnale `languageChanged` oppure `update_language` non aggiorna gli elementi. | Verificare il collegamento in `__init__`: `language_manager.languageChanged.connect(self.update_language)`. |
| Il tema non si applica. | Non viene chiamato `update_theme` alla creazione del nodo o al cambio tema. | In `MainWindow`, dopo aver creato il nodo, invocare `node.update_theme(self.theme_manager.current_theme())`. |
| La freccia punta al centro del nodo. | Si e fatto clic sul corpo invece che sul cerchio, oppure il nome non coincide con `PORTS`. | Fare clic direttamente sul cerchio. Verificare che `get_port_scene_pos` abbia la mappatura degli alias. |
| `NameError: name 'self' is not defined` all'importazione. | Gli attributi di istanza sono stati dichiarati fuori da `__init__`. | Tutti gli attributi come `self.mio_parametro` devono essere definiti all'interno di `__init__`. |
| I parametri si perdono alla copia/apertura di `.sflow`. | Il nodo non eredita da `SerializableMixin` oppure `SERIALISABLE` non e definito. | Implementare il passo 3.6 di questa guida. |

---

## 8. Conclusione

Seguendo questa guida e utilizzando il template `template_node.py`, potrai aggiungere nuovi nodi a FloWorks in modo efficiente e coerente con il resto del sistema. Ricorda sempre di mantenere la compatibilita con i18n e i temi per un'esperienza utente professionale.

Sii incoraggiato a contribuire con i tuoi nodi!
