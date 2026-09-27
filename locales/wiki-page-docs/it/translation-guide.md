---
title: Guida all'Internazionalizzazione (i18n)
description: Istruzioni passo passo per aggiungere e gestire le traduzioni in FloWorks
---

# 🌐 Guida all'Internazionalizzazione (i18n)

Questo documento spiega come aggiungere una nuova lingua a FloWorks e gestire efficacemente i file di traduzione.

---

## ➕ Come Aggiungere una Nuova Lingua

### Passo 1: Creare il File JSON
Passa alla cartella `locales/`. Copia `en.json` e rinominalo usando il corrispondente codice di due lettere [ISO 639-1](https://it.wikipedia.org/wiki/ISO_639-1) (es. `fr.json` per il francese, `de.json` per il tedesco).

### Passo 2: Tradurre le Stringhe
Apri il nuovo file JSON in un editor di testo.

!!! warning "Non Modificare le Chiavi"
    **Non cambiare mai le chiavi** (il lato sinistro di ogni coppia). Traduci solo i valori (il lato destro).

**Originale (`en.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "File",
    "edit": "Edit"
  }
}
```

**Esempio Tradotto (`es.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "Archivo",
    "edit": "Editar"
  }
}
```
Assicurati che la chiave radice `"language_name"` contenga il nome nativo della lingua (es. `"Français"`, `"Deutsch"`, `"Español"`).

### Passo 3: Validare il JSON
Verifica che il file sia un JSON valido (nessuna virgola finale, virgolette corrette, escape appropriati). Puoi usare validatori online come [JSONLint](https://jsonlint.com) o eseguire:
```bash
python -m json.tool locales/es.json
```

### Passo 4: Testare la Nuova Lingua
1. Avvia FloWorks.
2. Vai su **INFO → Lingua** e seleziona la nuova lingua.
3. Verifica che tutti gli elementi dell'interfaccia si aggiornino immediatamente (menu, pannelli, finestre di dialogo, etichette dei nodi, ecc.).

### Passo 5: Rilevamento Automatico (Opzionale)
Se le impostazioni locali del sistema dell'utente corrispondono al codice della nuova lingua, FloWorks la userà automaticamente al primo avvio (a condizione che non sia stata salvata una preferenza precedente in `QSettings`).

---

## 🌍 Lingue Disponibili
- **Inglese** (`en`) – Lingua di base / di fallback
- **Spagnolo** (`es`)

---

## ⚙️ Note Importanti e Buone Pratiche

!!! info "Meccanismo di Fallback"
    La lingua di base è l'**inglese**. Se manca una chiave di traduzione in un file lingua, FloWorks usa automaticamente la stringa in inglese come fallback.

!!! warning "Prevenzione dell'Overflow dell'UI"
    Mantieni le traduzioni concise per evitare rotture del layout. Se un testo tradotto è significativamente più lungo, considera di abbreviare o affidarti al sistema dei temi per la gestione del ridimensionamento dinamico.

!!! tip "Preservare HTML e Placeholder"
    - **Tag HTML:** Conserva tutti i tag HTML esattamente come sono (es. `<h3>`, `<b>`, `<pre>`, `<br>`).
    - **Placeholder:** Mantieni la sintassi `{variable}` dove viene usata (es. `"Idioma cambiado a: {name} ({code})"`). Non riordinarli o eliminarli.

---

## 🔗 Documentazione Correlata
- [📖 Mappa del Codice e Architettura](architecture-ii.md)
- [📦 Guida di Build e Distribuzione](build.md)
- [🧩 Riferimento Nodi](node-reference.md)
