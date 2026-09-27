# Filosofia

## Visione generale

FloWorks è un'applicazione desktop che consente di creare catene di elaborazione dei segnali mediante diagrammi di flusso visivi.
Trascinate, connettete e configurate i nodi; il risultato viene calcolato e visualizzato in tempo reale.
Lavorate con segnali simulati o collegate strumenti reali (oscilloscopi, generatori, multimetri LCR) senza necessità di scrivere codice, sebbene sia disponibile un potente ambiente di scripting se desiderate estendere le funzionalità.

---

## Caratteristiche principali

- **Diagrammi interattivi** – Costruite il vostro flusso di lavoro unendo nodi con linee che rappresentano il flusso dei dati.
- **Elaborazione in tempo reale** – Ogni modifica si riflette immediatamente nei grafici e nelle visualizzazioni.
- **Simulazione e hardware reale** – Generate segnali di prova o acquisite dati direttamente da strumenti di laboratorio.
- **Nodo script avanzato** – Incorporate il vostro codice Python con aiuto all'autocompletamento, parametri dinamici modificabili e memoria persistente tra le esecuzioni.
- **Visualizzazione professionale** – Segnali, spettri, spettrogrammi e grafici di alta qualità pronti per l'esportazione.
- **Multilingue** – L'interfaccia rileva la lingua del sistema e consente di passare tra spagnolo, inglese e altre lingue in qualsiasi momento.
- **Temi visivi** – Modalità scura, chiara e ad alto contrasto per adattarsi alle vostre preferenze o esigenze di accessibilità.
- **Gestione completa dei progetti** – Salvate il vostro lavoro in file `.sflow` e recuperatelo esattamente come lo avete lasciato, con annulla e ripristino illimitati.

---

## Come lavorare con FloWorks

### Nodi
Un nodo è un pezzo dell'elaborazione. Sono organizzati in tre categorie:

- **Sorgenti** – Inseriscono segnali all'inizio del flusso. Per esempio, un oscilloscopio (reale o simulato), un generatore di funzioni o un'operazione matematica.
- **Elaborazione** – Trasformano i dati. Somme, sottrazioni, condizionali, filtri… incluso un nodo speciale per scrivere i propri script in Python.
- **Pozzi** – Mostrano o esportano i risultati. Il visualizzatore grafico e l'esportatore professionale di grafici sono i più utilizzati.

### Connessioni
Le unioni tra i nodi sono disegnate come curve morbide o linee ortogonali. Un'animazione del flusso vi indica in ogni momento la direzione dei dati. Il sistema organizza automaticamente i cavi in modo che non si sovrappongano.

### Visualizzazione
Ogni volta che un nodo produce un segnale, questo può essere visto nel pannello grafico integrato. Potete esplorare diverse rappresentazioni (forma d'onda, spettro, spettrogramma) e regolare la scala con il mouse.

---

## Nodi in evidenza
Sono i nodi minimi indispensabili, necessari affinché la filosofia del programma abbia senso.

### Nodo generatore di segnali
Sorgente di segnali che può generare simulazioni personalizzate di forme d'onda a piacere dell'utente. Permette da un menu contestuale di selezionare o digitare la forma d'onda richiesta.

### Nodo di script
Un ambiente di programmazione completo all'interno del diagramma:

- **Editor con evidenziazione della sintassi**, autocompletamento e console degli errori.
- **Parametri dinamici** – Definite variabili modificabili dal pannello del nodo senza modificare il codice.
- **Porte configurabili** – Aggiungete ingressi e uscite aggiuntivi direttamente dall'editor.
- **Stato persistente** – Salvate valori tra le esecuzioni; tutto è memorizzato insieme al progetto.

### Esportatore di grafici
Nodo pozzo che genera immagini di alta qualità per relazioni o pubblicazioni. Permette di configurare dimensione, risoluzione, formato, tra gli altri.

---

## Personalizzazione

- **Lingua** – L'applicazione rileva automaticamente la lingua del sistema e salva la preferenza. Potete cambiarla dal menu senza riavviare.
- **Aspetto** – Scegliete tra tema scuro, chiaro o ad alto contrasto in base alla luce ambientale o alle vostre esigenze visive.

---

## Progetti e file

Salvate il vostro diagramma completo in un file `.sflow`.
All'apertura recupererete tutti i nodi, le connessioni, gli script, i parametri e le configurazioni di visualizzazione.
Le azioni di annulla e ripristino vi permettono di sperimentare senza timore di perdere il lavoro precedente.

---

FloWorks è progettato perché vi concentriate sull'analisi dei segnali e non sui dettagli tecnici dell'implementazione. Trascinate, connettete e scoprite.
