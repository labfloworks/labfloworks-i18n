# Note adesive (Sticky Notes) – Guida utente

## Cosa sono le note adesive?

Le note adesive (o *sticky notes*) sono piccoli blocchi di testo che puoi posizionare liberamente sul diagramma. Servono per:

- Aggiungere promemoria, titoli o spiegazioni direttamente sulla Tela.
- Creare tutorial passo passo che guidino chi utilizza il tuo progetto.
- Documentare parti del flusso di lavoro senza uscire da FloWorks.
- Lasciare commenti per te stesso o per altri collaboratori.

Le note sono ridimensionabili (trascinandone gli angoli), si spostano in qualsiasi punto del diagramma e vengono salvate insieme al progetto. All'apertura di un file `.sflow`, tutte le note appaiono esattamente dove le hai lasciate.

---

## La novità: note multilingue

Le note adesive possono mostrare automaticamente il testo nella lingua che scegli per l'applicazione.  
Invece di scrivere il messaggio finale in una sola lingua, puoi inserire **marcatori speciali** che si tradurranno da soli al cambio della lingua di FloWorks.

In questo modo, una stessa nota può essere letta in spagnolo, inglese o qualsiasi altra lingua disponibile senza bisogno di modificare il testo ogni volta.

---

## Come scrivere una nota multilingue

All'interno di una nota (creala con un doppio clic o con il pulsante 📝 della Barra degli strumenti) puoi usare due tipi di marcatori:

### 1. Con la parola `tr(…)`
Scrivi `tr("chiave")` e sostituisci `chiave` con un nome descrittivo della frase.

Esempio:

```
tr("tutorial.paso1.titulo")
tr("tutorial.paso1.mensaje")
```

### 2. Con doppie graffe `{{…}}`
Scrivi `{{chiave}}` allo stesso modo.

Esempio:

```
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Entrambi i formati funzionano allo stesso modo; scegli quello che ti è più comodo (puoi anche combinarli nella stessa nota).

> **Importante**: Il testo che vedi quando modifichi la nota contiene i marcatori originali (ad es. `{{tutorial.passo1.titolo}}`).  
> Al termine della modifica e al ritorno alla visualizzazione normale del diagramma, i marcatori vengono sostituiti dalla frase tradotta nella lingua corrente dell'applicazione.

---

## Comportamento al cambio di lingua

- Se cambi la lingua dal menu di FloWorks (ad esempio dallo spagnolo all'inglese), **tutte le note adesive che contengono marcatori si aggiornano automaticamente**.
- Non è necessario chiudere e riaprire il progetto, né modificare manualmente ogni nota.
- Le note che contengono solo testo normale (senza marcatori) non sono interessate; mostrano lo stesso contenuto in qualsiasi lingua.

---

## Vantaggi dell'uso dei marcatori

- **Tutorial multilingue istantanei** – Una sola nota può servire a guidare utenti di diverse lingue.
- **Coerenza** – Se modifichi la traduzione in un unico punto (il file delle lingue mantenuto dal tuo team di sviluppo), tutte le note che usano quella chiave si aggiorneranno.
- **Manutenzione semplice** – Puoi scrivere il contenuto una sola volta e riutilizzarlo in più note.
- **Flessibilità** – Combina testo fisso con marcatori. Ad esempio:

```
🎯 PASSO 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

---

## Esempio pratico: un tutorial passo passo

Supponi di voler aggiungere una nota che spieghi il primo passo di un tutorial.  
In modalità modifica scrivi:

```
🎯 PASSO 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Al termine della modifica e usando l'applicazione in spagnolo, vedrai:

```
🎯 PASO 1
¡Bienvenido a FloWorks!
Arrastre un nodo fuente de señal para comenzar.
```

Se cambi la lingua in inglese, la stessa nota mostrerà:

```
🎯 STEP 1
Welcome to FloWorks!
Drag a signal source node to begin.
```

E così via per qualsiasi altra lingua configurata.

---

## Riepilogo

- Le note adesive arricchiscono i tuoi diagrammi con informazioni testuali.
- Ora possono essere **multilingue** usando i marcatori `tr("chiave")` o `{{chiave}}`.
- In modifica vedi le chiavi; in visualizzazione, il testo tradotto.
- Cambia la lingua dell'applicazione e tutte le note si adatteranno all'istante.
- Perfette per creare documentazione visiva, tutorial o avvisi che devono funzionare in più lingue.

Sfrutta questa funzionalità per rendere i tuoi progetti più accessibili e facili da condividere!
