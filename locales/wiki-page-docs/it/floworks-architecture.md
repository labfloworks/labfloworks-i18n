---
title: Architettura di FloWorks
description: Panoramica dei componenti e del funzionamento interno per l'utente finale
---

# Architettura di FloWorks – Visione per l'utente

FloWorks è un'applicazione desktop che consente di costruire catene di elaborazione dei segnali mediante diagrammi di flusso. Collegate blocchi (nodi) su una tela interattiva e visualizzate i risultati in tempo reale. Per rendere questo possibile, l'applicazione è organizzata in diversi moduli che lavorano insieme. Di seguito si spiega, senza dettagli tecnici, cosa fa ogni parte e come si relazionano tra loro.

---

## Struttura generale

L'applicazione si compone delle seguenti aree funzionali:

| Area | Cosa fa? |
|------|------------|
| **Avvio e finestra principale** | Avvia il programma, mostra la finestra, i menu e coordina tutte le azioni dell'utente. |
| **Motore di esecuzione** | Calcola l'ordine in cui i nodi devono essere eseguiti, rileva dipendenze e cicli, e trasmette i dati da un nodo all'altro. |
| **Scena e diagramma** | Gestisce la tela su cui si collocano i nodi, le connessioni tra essi, le note adesive e le azioni di annulla/ripeti. |
| **Nodi ed elaborazione** | Contiene tutti i tipi di blocchi utilizzabili: sorgenti di segnale, operazioni matematiche, script personalizzati, esportazione grafici, ecc. |
| **Connettori visivi** | Disegna le linee che uniscono i nodi (curve morbide o ortogonali), le anima per mostrare il flusso dei dati ed evita sovrapposizioni. |
| **Interfaccia utente** | Include la vista del diagramma (zoom, scorrimento), la barra degli strumenti, la tabella dei parametri, i pannelli di analisi (statistiche, cursori) e le finestre di configurazione. |
| **Supporto hardware reale** | Permette la comunicazione con strumenti di laboratorio (oscilloscopi, generatori, multimetri LCR) per acquisire o generare segnali reali. |
| **Esportazione grafici** | Genera immagini di alta qualità (PNG, PDF, SVG) con piena personalizzazione visiva. |
| **Temi e aspetto** | Cambia l'aspetto dell'intera applicazione (scuro, chiaro, alto contrasto) e consente di regolare la dimensione del carattere. |
| **Lingue** | Traduce l'intera interfaccia in più lingue e consente di cambiare lingua all'istante. |
| **Gestione progetti** | Salva e apre file `.sflow` con l'intero diagramma, incluse configurazioni, script e risultati. |
| **Test e diagnostica** | Strumenti interni per verificare il corretto funzionamento (non visibili all'utente finale). |

---

## Come funziona internamente

### Avvio e finestra principale
All'apertura di FloWorks, l'ambiente grafico viene configurato, viene rilevata la densità di pixel dello schermo (in modo che tutto appaia nitido su monitor 4K o normali) e viene mostrata la finestra principale. Questa finestra centralizza tutti gli elementi: l'area di disegno, i menu, la barra degli strumenti e i pannelli laterali.

### Motore di flusso
Quando si preme "Esegui" (o si preme F5), un motore interno attraversa tutti i nodi nell'ordine corretto, rispettando le connessioni. Sa quali nodi dipendono da altri e previene cicli infiniti. Supporta nodi che ricevono più ingressi con nome e che producono più uscite. I dati viaggiano tra i nodi senza perdere la loro struttura originale.

### Scena del diagramma
La tela su cui si costruiscono i diagrammi è una scena intelligente:

- Permette di aggiungere, spostare, collegare e selezionare i nodi.
- Supporta annulla e ripeti illimitati per qualsiasi azione.
- Include note adesive ridimensionabili che si possono posizionare liberamente e che vengono salvate con il progetto.
- Dispone di un organizzatore automatico che riposiziona i nodi ordinatamente (con Ctrl+Shift+L).
- Al salvataggio, l'intero diagramma viene impacchettato in un file `.sflow` che contiene le descrizioni dei nodi, le connessioni, le note e i dati numerici associati.

### Connettori
Le linee che uniscono i nodi sono disegnate come curve morbide o percorsi ortogonali. Un'animazione sottile di punti o trattini indica la direzione del flusso. Un gestore di corsie evita che più connessioni tra gli stessi nodi si sovrappongano; le separa automaticamente in modo che tutto sia leggibile.

### Tipi di nodi
I nodi sono i mattoni fondamentali. Si raggruppano in tre categorie:

- **Sorgenti** – Generano segnali. Possono simulare onde (sinusoidale, quadra, ecc.) o leggere dati reali da un oscilloscopio o multimetro connesso. Supportano più canali simultanei (ad esempio, impedenza e fase da un LCR).
- **Elaborazione** – Trasformano i dati. Includono operazioni aritmetiche (somma, sottrazione, moltiplicazione, divisione), decisioni condizionali (diramazione Sì/No) e un potente nodo script che consente di scrivere il proprio codice Python con supporto visivo.
- **Pozzi** – Mostrano o esportano i risultati. Il più comune è il visualizzatore grafico (oscilloscopio virtuale), ma esiste anche un esportatore di grafici di qualità professionale.

Ogni nodo ha porte di ingresso (sinistra/alto) e di uscita (destra/basso). Collegando una porta di uscita a una di ingresso, il segnale fluisce tra di esse.

#### Nodo script avanzato
Il nodo script merita una menzione speciale. È pensato per utenti avanzati che vogliono aggiungere la propria elaborazione senza uscire da FloWorks. Offre:

- Un editor con evidenziazione della sintassi, autocompletamento e numerazione delle righe.
- La possibilità di definire parametri modificabili dal pannello del nodo senza toccare il codice (ad esempio, un valore numerico poi usato nello script).
- Porte di ingresso e uscita dinamiche: aggiungendo commenti speciali nello script, si possono creare nuovi connettori.
- Memoria persistente: una variabile speciale (`persist`) che conserva il suo valore tra le esecuzioni, utile per accumulatori o macchine a stati.
- Modelli di script già pronti e l'opzione di salvare i propri.
- Un sistema di aiuto integrato e una console che mostra gli errori di esecuzione.

### Interfaccia utente
Oltre alla tela, l'interfaccia include:

- Una **barra degli strumenti** con tutti i nodi organizzati per categorie, menu di lingua, tema e dimensione carattere, e accesso al visualizzatore dei log.
- Una **tabella dei parametri** che mostra informazioni sui nodi selezionati ed evidenzia possibili incompatibilità (come tentare di operare con segnali di lunghezza diversa).
- **Pannelli di analisi** ancorabili: statistiche (massimo, minimo, valore efficace), cursori A/B per misurare differenze, e un mirino con marcatore di picco.
- Una **finestra di benvenuto** che si adatta alla risoluzione dello schermo e offre opzioni iniziali.

### Connessione con strumenti reali
Se si dispone di hardware compatibile (oscilloscopi Siglent SDS, multimetri LCR, generatori SDG), FloWorks può comunicare con essi tramite il protocollo standard VISA/SCPI. La configurazione avviene da pannelli specifici all'interno dell'applicazione. Quando si acquisisce un segnale multicanale (ad esempio, modulo e fase da un LCR), il nodo sorgente impacchetta tutti i canali e si può scegliere quale visualizzare con un semplice menu contestuale.

### Esportazione grafici professionale
Il nodo esportatore di grafici permette di generare immagini pronte per report o pubblicazioni. Facendo doppio clic su di esso si apre una finestra con molte opzioni: si possono personalizzare colori, tipi di linea, etichette, scale, scegliere tra i formati PNG, PDF o SVG, e salvare le preferenze come profili riutilizzabili.

### Personalizzazione visiva
FloWorks include vari temi (scuro, chiaro, alto contrasto) che cambiano l'aspetto dell'intera applicazione all'istante, senza riavvio. Inoltre, si può regolare la dimensione del carattere globale dal menu (Informazioni → Dimensione carattere) e tutti gli elementi si ridimensionano di conseguenza, inclusi i testi all'interno dei nodi, le note adesive e i grafici.

### Sistema di lingue
L'applicazione rileva automaticamente la lingua del sistema al primo avvio e salva la preferenza. È possibile cambiare lingua in qualsiasi momento dal menu; tutti i testi, i menu e gli aiuti si aggiornano al volo.

### Progetti e file `.sflow`
Tutto il lavoro viene salvato in un unico file con estensione `.sflow`. Questo file contiene l'intero diagramma: nodi, connessioni, note, configurazioni, script e i dati numerici generati. Si può condividere con altri utenti; all'apertura su un altro computer, le note e i nodi si riscaldano automaticamente alla densità di pixel di quello schermo.

---

## Flussi di lavoro tipici

1. **Creare un diagramma semplice**  
   Selezionare un nodo sorgente (ad es., Generatore) e un nodo Visualizzatore dalla barra degli strumenti.  
   Collegare l'uscita del generatore all'ingresso del visualizzatore (Ctrl+clic sulla porta di uscita, poi clic su quella di ingresso).  
   Premere F5 per eseguire. Il segnale apparirà nel grafico.

2. **Usare uno script personalizzato**  
   Aggiungere un nodo Script.  
   Scrivere il proprio codice Python nell'editor; si possono definire parametri modificabili e porte extra.  
   Collegare i suoi ingressi e uscite come per qualsiasi altro nodo.  
   Eseguire il flusso; lo script verrà elaborato con i propri dati.

3. **Acquisire dati da un oscilloscopio reale**  
   Collegare lo strumento e configurare la comunicazione dal pannello del nodo Oscilloscopio.  
   Il nodo acquisisce il segnale e lo fornisce attraverso le sue porte di uscita (una per canale).  
   Collegare queste porte ad altri nodi di elaborazione o al visualizzatore.

4. **Esportare un grafico per una relazione**  
   Collegare il segnale desiderato a un nodo Esportatore di grafici.  
   Selezionare nel nodo (clic destro) per configurare l'aspetto visivo del grafico.  
   È anche possibile caricare/salvare profili per velocizzare l'ottenimento di grafici pronti per le relazioni, ottenendo il file immagine nell'estensione scelta.

---

## A cosa serve tutto questo

Questa architettura è pensata per permettervi di concentrarvi sull'analisi dei segnali senza preoccuparvi di come è organizzato internamente il programma. Ogni componente ha una funzione chiara e lavora insieme agli altri per offrire un'esperienza fluida, dalla simulazione alla strumentazione reale, passando per la personalizzazione visiva e l'esportazione dei risultati.

Se mai doveste ampliare le capacità di FloWorks (ad esempio, aggiungendo nuovi tipi di nodi o collegando uno strumento diverso), sappiate che esiste una struttura modulare che lo consente, anche se quello è territorio per sviluppatori. Come utente finale, godetevi la flessibilità che questo design vi offre.
