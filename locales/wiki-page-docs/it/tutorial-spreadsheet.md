# 📊 Tutorial del foglio di calcolo

Foglio di calcolo di base in stile Excel, leggero e incorporabile.

---

## 1. Cos'è Spreadsheet?

È un componente foglio di calcolo che offre:

- Celle con supporto per formule (iniziano con `=`)
- Funzioni predefinite (SOMMA, MEDIA, CONDIZIONALI, etc.)
- Operatori aritmetici, logici e di confronto
- Interfaccia minimalista, ideale per l'integrazione in applicazioni Qt

---

## 2. Navigazione di base

- Fai clic su una cella per selezionarla.
- Scrivi direttamente per inserire testo o numeri.
- Per scrivere una **formula**, inizia con `=` (es. `=SUM(A1:A5)`).
- Premi **Invio** per confermare la modifica.
- Usa i tasti freccia o il mouse per spostarti.

---

## 3. Formule: funzioni disponibili

Tutte le funzioni si scrivono in maiuscolo e ammettono intervalli (es. `A1:A5`) o argomenti separati da virgola.

| Funzione | Cosa fa | Esempio |
| -------- | ------- | ------- |
| `SUM` | Somma numeri | `=SUM(A1:A5)` |
| `AVG` / `AVERAGE` | Media | `=AVG(A1:A5)` |
| `COUNT` | Conta numeri (non vuoti) | `=COUNT(A1:A5)` |
| `MAX` | Valore massimo | `=MAX(A1:A5)` |
| `MIN` | Valore minimo | `=MIN(A1:A5)` |
| `ABS` | Valore assoluto | `=ABS(A1)` |
| `ROUND` | Arrotonda (2° argomento = decimali) | `=ROUND(A1, 2)` |
| `IF` | Condizionale (se, allora, altrimenti) | `=IF(A1>10, "Sì", "No")` |
| `CONCAT` / `CONCATENATE` | Unisce testi | `=CONCAT(A1, " ", B1)` |
| `LEN` | Lunghezza testo | `=LEN(A1)` |
| `INT` | Parte intera | `=INT(A1)` |
| `SQRT` | Radice quadrata | `=SQRT(A1)` |

### 3.1. Note sulle funzioni

- Gli intervalli si indicano con **due punti**: `A1:A5` include tutte le celle da A1 ad A5.
- Le funzioni possono essere nidificate: `=SUM(A1:A5) + MAX(B1:B5)`.
- Gli argomenti di testo devono essere tra virgolette doppie o singole.

---

## 4. Operatori senza funzioni

Oltre alle funzioni, puoi usare operatori direttamente nella formula. La sintassi è simile a Python.

### 4.1. Aritmetici

| Operazione | Esempio |
| ---------- | ------- |
| Addizione | `=A1+A2+A3` |
| Sottrazione | `=A1-A2` |
| Moltiplicazione | `=A1*B1` |
| Divisione | `=A1/B1` |
| Modulo | `=A1%B1` |
| Potenza | `=A1**2` (o `=A1^2`) |

### 4.2. Confronti

Restituiscono `True` o `False` (mostrati come `Vero` / `Falso`).

| Operatore | Significato | Esempio |
| --------- | ----------- | ------- |
| `>` | Maggiore di | `=A1>B1` |
| `<` | Minore di | `=A1<B1` |
| `>=` | Maggiore o uguale | `=A1>=10` |
| `<=` | Minore o uguale | `=A1<=10` |
| `==` | Uguale | `=A1==B1` |
| `!=` | Diverso | `=A1!=B1` |

### 4.3. Logici e condizionali

Puoi combinare condizioni con `and`, `or`, `not`.

```excel
= A1>5 and B1<10
= not(A1==0)
= 10 if A1>5 else 0
```

L'operatore ternario `if` `else` è supportato direttamente.

---

## 5. Esempi pratici

### 5.1. Somma delle vendite

Supponi di avere le vendite in `B2:B10` e di volere il totale:

```excel
=SUM(B2:B10)
```

### 5.2. Sconto condizionale

Se il totale (in `B12`) supera 100, applica uno sconto del 10%; altrimenti 0:

```excel
= IF(B12>100, B12*0.9, B12)
```

### 5.3. Media e conta

Media dei voti in `C2:C20`, ma solo se ci sono almeno 5 valori:

```excel
= IF(COUNT(C2:C20)>=5, AVG(C2:C20), "Dati insufficienti")
```

### 5.4. Testo combinato

Unire nome (A2) e cognome (B2) con uno spazio:

```excel
= CONCAT(A2, " ", B2)
```

### 5.5. Radice quadrata di un numero

```excel
= SQRT(A1)
```

### 5.6. Arrotondamento a 2 decimali

```excel
= ROUND(A1, 2)
```

---

## 6. Suggerimenti e trucchi

- **Riferimenti relativi/absoluti:** per ora, tutti i riferimenti sono relativi (come in Excel). `$A$1` non è ancora supportato.
- **Intervalli dinamici:** puoi usare intervalli come `A:A` (intera colonna) o `1:1` (intera riga).
- **Autocompletamento:** scrivendo `=`, appare un menu con le funzioni disponibili.
- **Errori:** se una formula non è valida, la cella mostra `#ERROR` e il messaggio dettagliato nella barra di stato.
- **Ricalcolo:** le formule si aggiornano automaticamente alla modifica delle celle dipendenti.

---

## 8. Domande frequenti

**Come si esportano i dati?**
Per ora non c'è un'esportazione nativa, ma puoi accedere ai dati tramite il modello interno.

**Supporta i grafici?**
No, è un foglio di calcolo di base. Puoi combinarlo con altri widget per la visualizzazione.

---

Buon uso del foglio di calcolo leggero!

© 2026 — FloWorks
