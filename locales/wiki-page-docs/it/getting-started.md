---
title: Per iniziare con FloWorks
description: Guida rapida per configurare l'ambiente, eseguire il primo flusso e accedere alla versione portatile.
---

# 🚀 Per iniziare con FloWorks

Questa guida vi porterà da zero all'esecuzione del vostro primo flusso di elaborazione dei segnali. FloWorks è un'applicazione per diagrammi di flusso per segnali, costruita con Python e PySide6, che supporta hardware reale (VISA/SCPI), simulazione integrata, scripting avanzato e cambio di lingua al volo.

---

## 🌊 Il vostro primo flusso di esempio

Creiamo un flusso semplice: generiamo un segnale sinusoidale e lo visualizziamo in tempo reale.

1. **Aggiungere nodi**
   Nella barra degli strumenti superiore, selezionate `Sorgente` → selezionate `Generatore di segnali Avanzato`. Quindi, da `Elaborazione` → selezionate, ad esempio, `Spettrale`.
2. **Collegare**
   Fate `Ctrl+Clic` sulla porta di uscita (`destra`) del generatore. Poi, fate clic sulla porta di ingresso (`sinistra`) dell'oscilloscopio. O semplicemente fate clic sulla porta di uscita e trascinate (tenendo premuto) fino alla porta di ingresso del nodo successivo.
3. **Configurare (opzionale)**
   Fate clic su un nodo, nel pannello laterale sinistro apparirà un editor con i parametri del nodo selezionato per regolare le condizioni operative. Nella parte inferiore c'è una visualizzazione grafica che rappresenta visivamente i dati generati o acquisiti dai nodi.
4. **Eseguire**
   Premete `F5` o il pulsante ▶ sulla barra degli strumenti. Il motore topologico calcolerà l'ordine di esecuzione, elaborerà i dati e vedrete l'onda nel pannello grafico. **Il connettore si animerà indicando il flusso attivo!**

---

## 🧠 Capire le porte: categorie per colore

In FloWorks, ogni porta appartiene a una **categoria funzionale** identificata da un colore. Le connessioni valide si fanno **sempre tra porte dello stesso colore**: un'uscita di una categoria si collega esclusivamente con un ingresso della stessa categoria. Inoltre, la linea connettore assume automaticamente il colore delle porte che unisce, facilitando la lettura visiva.

| Tipo | Colore | Scopo | Esempio tipico |
|------|-------|-----------|----------------|
| `control` | Bianco | Flusso di controllo / attivazione. | Segnale di avvio verso un nodo di acquisizione. |
| `exec` | Grigio | Esecuzione di operazioni o passi. | Innesco di una funzione o callback. |
| `data` | Verde | Dati generici / segnali numerici. | Uscita di un generatore o sensore. |
| `int` | Blu | Numeri interi. | Indice, dimensione buffer, ID. |
| `float` | Ciano | Numeri in virgola mobile. | Ampiezza, frequenza, soglia. |
| `string` | Viola | Stringhe di testo. | Nome file, etichetta. |
| `bool` | Rosa | Valori booleani (`True`/`False`). | Flag di stato, abilitazione. |
| `array` | Blu scuro | Array / vettori. | Segnale multicanale, lista di campioni. |
| `trigger` | Arancione | Trigger / eventi discreti. | Impulso di sincronizzazione, fronte. |

**Regola d'oro:**

- Si collegano solo porte dello **stesso colore esatto** (uscita ↔ ingresso della stessa categoria).
- Il sistema evita connessioni non valide ed evidenzia visivamente le porte compatibili durante il trascinamento.
- La linea connettore prende il colore delle porte collegate; così ogni percorso si identifica a colpo d'occhio.

**Filosofia FloWorks:**
Le porte dati **preservano la dimensionalità** degli array. Non si applica mai un appiattimento automatico: se entra una matrice, esce una matrice, mantenendo l'integrità dei vostri segnali multidimensionali.

![FloWorks](assets/tipos_de_puertos.PNG)

---

## 🖱️ Navigazione sulla tela (Canvas)

Padroneggiate lo spazio di lavoro con questi gesti:

| Azione | Come farla |
|--------|--------------|
| **Zoom** | Rotella del mouse o `Ctrl + rotella` |
| **Pan (scorrimento)** | Tenete premuto `Spazio` e trascinate, oppure usate il pulsante centrale del mouse |
| **Selezionare un nodo** | Clic sinistro sul nodo |
| **Selezione multipla** | Trascinate un rettangolo con clic sinistro, oppure `Ctrl + clic` su più nodi |
| **Spostare la selezione** | Trascinate uno qualsiasi dei nodi selezionati |
| **Aprire configurazione** | Doppio clic sul nodo |

**Suggerimento:** Il pannello sinistro si aggiorna automaticamente con la configurazione del nodo selezionato, senza bisogno di aprire finestre aggiuntive.

---

## ⚡ Scorciatoie da tastiera e mosse avanzate

Queste scorciatoie trasformano un utente normale in un **power user**:

| Scorciatoia | Azione |
|-------|--------|
| `F5` | Esegui flusso |
| `Ctrl + S` | Salva progetto (`.sflow`) |
| `Ctrl + Clic` | Collega nodi (clic su porta di uscita → clic su porta di ingresso) |
| `Ctrl + C` / `Ctrl + V` | Copia / incolla nodi selezionati |
| `Ctrl + Z` / `Ctrl + Y` | Annulla / ripristina |
| `Ctrl + Shift + L` | Auto-organizza nodi sulla tela |
| `Canc` | Elimina nodi selezionati |
| `Ctrl + A` | Seleziona tutti i nodi |

**Mosse avanzate:**

- **Duplicare un flusso:** selezionate un gruppo di nodi, `Ctrl + C`, `Ctrl + V` e trascinate la copia in un'altra zona.
- **Pulire la griglia:** usate `Ctrl + Shift + L` per ordinare l'intera tela con un solo comando.
- **Connessione rapida:** `Ctrl + Clic` su una porta di uscita e poi clic normale sulla porta di ingresso; FloWorks disegna la connessione automaticamente.

---

## 🎨 Personalizzazione dell'ambiente

FloWorks si adatta a voi, non il contrario.

### Cambio tema al volo
Dalla barra superiore, menu **Visualizza → Tema**, scegliete tra chiaro, scuro o altri. L'interfaccia cambia **istantaneamente**, senza riavvio e senza perdere il flusso di lavoro.

### Dimensione carattere
In **Visualizza → Dimensione carattere** selezionate un valore predefinito o personalizzato. L'intera interfaccia si adatta al momento.

### Lingua
In **Visualizza → Lingua** selezionate la lingua desiderata. FloWorks supporta **cambio al volo**: menu, pulsanti e messaggi si traducono senza riavviare l'applicazione.

---

## ❗ Risoluzione dei problemi comuni

| Problema | Possibile causa | Soluzione |
|----------|---------------|----------|
| Il flusso non si esegue | Ci sono nodi non configurati o connessioni rotte | Controllate che tutti i nodi abbiano parametri validi e che le connessioni siano tra porte compatibili |
| Il grafico non si aggiorna | Il flusso è in pausa o non ci sono dati che scorrono | Assicuratevi di aver premuto `F5` o ▶, e che i nodi sorgente stiano generando dati |
| Non riesco a collegare due nodi | Le porte sono di tipo diverso | Verificate che entrambe le porte siano **dati** o entrambe **controllo** |
| Il programma va lento con flussi grandi | Troppi nodi o grafici in tempo reale | Chiudete i pannelli di analisi non usati o riducete la frequenza di campionamento dei nodi sorgente |
| Il tema non cambia | Alcuni widget potrebbero non essere registrati | Riavviate l'applicazione e riprovate (sarà risolto nelle versioni future) |

---

## 🧪 Esempi pratici rapidi

Oltre al flusso sinusoidale iniziale, provate questi mini-progetti per padroneggiare FloWorks:

| Esempio | Nodi coinvolti | Risultato atteso |
|---------|-------------------|--------------------|
| **Filtro passa-basso** | Generatore → Filtro  Visualizzatore grafici | Vedrete il segnale filtrato |
| **Acquisizione simulata** | Generatore → Analizzatore THD | Valore della distorsione armonica del segnale |
| **Controllo manuale** | Generatore → Ispettore dati | Tabella con i valori del segnale inviato dal generatore |
| **Confronto segnali** | Due generatori → Sommatore → Visualizzatore grafici | Il risultato dell'operazione (somma, sottrazione, moltiplicazione o divisione) di due onde in un solo grafico |

Ognuno di questi flussi si può montare in meno di un minuto, dimostrando l'agilità di FloWorks rispetto alla codifica tradizionale.

---

## 📚 Cosa c'è dopo?

| Risorsa | Descrizione |
|---------|-------------|
| [🗺️ Guida all'Anatomia dell'Interfaccia Principale](interface-anatomy.md) | Comprensione dell'architettura e della filosofia dell'interfaccia grafica |
| [🗺️ Mappa del Codice e dell'Architettura](philosophy.md) | Struttura completa, manager, contratti e DPI-Awareness. |
| [🧩 Riferimento Tecnico dei Nodi](node-reference.md) | Catalogo, `ScriptNode`, multicanale e come estendere il sistema. |
| [🌐 Guida all'Internazionalizzazione](translation-guide.md) | Aggiungere lingue, validare JSON e gestire le chiavi `tr()`. |
| [📦 Guida al Build Portatile](guia-ejecutable-portable.md) | PyInstaller, hook, `--onefile`, risoluzione errori e firma digitale. |

---

!!! warning "Note di compatibilità e uso"
    1. **Versione Python:** Potete usare 3.9+ e sistemi a 64 bit.
    2. **Firewall di Windows:** Se usate hardware reale (oscilloscopio VISA/SCPI), consentite `FloWorks.exe` nel firewall. L'app mostra una finestra personalizzata se la connessione è bloccata (la finestra del SO non appare in modalità `--windowed`).
    3. **Scorciatoie chiave:** `F5` (esegui), `Ctrl+S` (salva `.sflow`), `Ctrl+Clic` (collega), `Spazio+clic` (pan libero), `Ctrl+Shift+L` (auto-layout).
    4. **Preservazione dati:** Il motore **non applica mai** `flatten()` agli array. Lavorate con copie locali se avete bisogno di vettorizzare.
