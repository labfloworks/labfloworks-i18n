---
title: Riferimento tecnico nodi
description: Catalogo aggiornato, contratti di estensione e capacità avanzate del sistema di nodi di FloWorks
---

# 🧩 Riferimento tecnico nodi

FloWorks non dipende da un catalogo statico. Utilizza un **sistema di registrazione dinamica** basato su contratti chiari. Ciò consente di ampliare la piattaforma senza toccare il motore topologico. Di seguito è dettagliato il catalogo implementato, le capacità tecniche reali e il protocollo per estenderlo in modo sicuro.

---

## 📂 Categorie del nucleo

=== "📦 Vista per strati"
    <div class="grid cards" markdown>

    - **📥 Sorgenti/Input**
      Generano o catturano segnali iniziali. Supportano simulazione integrata, hardware reale (VISA/SCPI) e modalità multicanale.
    - **⚙️ Elaborazione**
      Trasformano, combinano o analizzano dati. Preservano dimensionalità e interpolano automaticamente quando necessario.
    - **🔀 Controllo/Flusso**
      Biforcano, iterano o condizionano l'Esecuzione. Includono supporto nativo per segnali di attivazione.
    - **🐍 Scripting/Avanzato**
      Eseguono codice Python dinamico con porte parametriche (`# @param`), porte dinamiche (`# @input`/`# @output`) e persistenza stato (`persist`).
    - **🔌 Hardware/Strumentazione**
      Interfacce per oscilloscopi, metri LCR e generatori.
    - **📤 Uscita/Export**
      Visualizzano, esportano o archiviano risultati. Supportano temi visivi, profili utente e formato professionale (PNG/PDF/SVG).

    </div>

---

## 📋 Catalogo tecnico implementato

| Nodo | Tipo | Responsabilità principale | Caratteristiche chiave |
|------|------|--------------------------|------------------------|
| `SumNode` | Elaborazione | Operatore aritmetico (+, -, *, /) per due ingressi. | Interpola automaticamente segnali di diversa risoluzione (FFTs). Preserva dimensionalità. |
| `RhombusNode` | Controllo | Condizionale (biforcazione Sì/No). | Due porte di uscita. Valuta condizione per soglia o logica booleana. |
| `TriggerNode` | Controllo | Iteratore/accumulatore con attivazione esterna. | Riceve `(x, y, "trigger")`. Accumula fino a N iterazioni ed emette risultato impilato/medio. |
| `ScriptNode` | Avanzato | Ambiente di scripting Python integrato. | QScintilla, autocompletamento, `# @param`, porte dinamiche, `persist`, template, console errori, interprete esterno con timeout. |
| `OscilloscopeNode` | Hardware | Cattura da oscilloscopi (SDS) o metri LCR. | Modalità simulazione, dialogo firewall integrato, **supporto multicanale** (`out_primary`, `out_secondary`), menu "Mostra canale". |
| `GeneratorNode` | Sorgente | Invia segnali a generatori (SDG) o simula uscite. | Configurazione modulazione/sweep, dialogo simulazione integrato. |
| `GraphExporterNode` | Uscita | Esportatore professionale di grafici. | Configurazione doppio clic, assi personalizzati, temi, profili salvati, ecc. |

---

## 🔍 ScriptNode: Capacità essenziali

> **🐍 Ambiente di scripting integrato**
>
> - **Editor di codice integrato:** Evidenziazione sintassi di base, numerazione righe e piegatura codice.
> - **Pannello parametri dinamici:** Direttive `# @param NOME : tipo = valore` iniettano controlli modificabili (spinbox, campo testo, ecc.) nel pannello laterale.
> - **Porte dinamiche:** `# @input nome` e `# @output nome` creano porte in tempo reale. Lo script riceve un dizionario `inputs` e restituisce `outputs`.
> - **Persistenza stato:** Dizionario globale `persist` che mantiene valori tra le Esecuzioni.
> - **Template e Import/Export:** Menu a discesa con script base. L'utente può salvare i propri script in `nodes/script_node/templates/` o importare/esportare file `.py` esterni.
> - **Console errori integrata:** Mostra errori di sintassi/esecuzione con la riga esatta evidenziata nell'editor.
> - **Aiuto e i18n:** Tooltip contestuali, pulsante `?` con guida rapida, e tutti i testi usano `tr()` per la traduzione.
> - **Interprete esterno con timeout:** Percorso configurabile (`# @python_path` o pulsante "Sfoglia…"). Esecuzione isolata con limite di tempo e fallback all'interprete interno.
> - **Serializzazione completa:** Salva script, parametri, porte dinamiche e stato `persist`. Al caricamento di un `.sflow`, ricostruisce automaticamente porte e parametri.

---

## 📚 Risorse correlate

- [📖 Mappa del codice e architettura](architecture-ii.md) → Responsabilità per modulo e flussi di lavoro.
- [🌐 Guida all'internazionalizzazione (i18n)](i18n.md) → Come aggiungere lingue e gestire chiavi `tr()`.
- [🛠️ Aggiungere un nuovo nodo (tutorial)](adding-a-new-node.md) → Passo dopo passo con esempi pratici.
- [📦 Guida build e distribuzione](build.md) → Impacchettamento PyInstaller, hooks e firme digitali.

---

💡 **Manca un nodo in questo catalogo?**
FloWorks è progettato per essere estensibile. Se ti serve un nodo che non esiste, crealo seguendo il contratto di `BaseNode` e registralo. La comunità e il futuro marketplace amplieranno continuamente l'ecosistema senza rompere la compatibilità.
