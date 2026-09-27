# 📊 Tutorial pentru foaie de calcul

Foaie de calcul de bază în stil Excel, ușoară și încorporabilă.

---

## 1. Ce este Spreadsheet?

Este o componentă de foaie de calcul care oferă:

- Celule cu suport pentru formule (încep cu `=`)
- Funcții predefinite (SUM, AVERAGE, CONDITIONALE, etc.)
- Operatori aritmetici, logici și de comparație
- Interfață minimalistă, ideală pentru integrare în aplicații Qt

---

## 2. Navigare de bază

- Faceți clic pe o celulă pentru a o selecta.
- Scrieți direct pentru a introduce text sau numere.
- Pentru a scrie o **formulă**, începeți cu `=` (ex. `=SUM(A1:A5)`).
- Apăsați **Enter** pentru a confirma editarea.
- Folosiți săgețile sau mouse-ul pentru a vă deplasa.

---

## 3. Formule: funcții disponibile

Toate funcțiile se scriu cu majuscule și acceptă intervale (ex. `A1:A5`) sau argumente separate prin virgulă.

| Funcție | Ce face | Exemplu |
| ------- | ------- | ------- |
| `SUM` | Sumează numerele | `=SUM(A1:A5)` |
| `AVG` / `AVERAGE` | Media | `=AVG(A1:A5)` |
| `COUNT` | Numără valorile (nevide) | `=COUNT(A1:A5)` |
| `MAX` | Valoarea maximă | `=MAX(A1:A5)` |
| `MIN` | Valoarea minimă | `=MIN(A1:A5)` |
| `ABS` | Valoarea absolută | `=ABS(A1)` |
| `ROUND` | Rotunjire (al 2-lea argument = zecimale) | `=ROUND(A1, 2)` |
| `IF` | Condițional (dacă, atunci, altfel) | `=IF(A1>10, "Da", "Nu")` |
| `CONCAT` / `CONCATENATE` | Concatenează texte | `=CONCAT(A1, " ", B1)` |
| `LEN` | Lungimea textului | `=LEN(A1)` |
| `INT` | Partea întreagă | `=INT(A1)` |
| `SQRT` | Rădăcina pătrată | `=SQRT(A1)` |

### 3.1. Note despre funcții

- Intervalele se indică cu **două puncte**: `A1:A5` include toate celulele de la A1 la A5.
- Funcțiile pot fi imbricate: `=SUM(A1:A5) + MAX(B1:B5)`.
- Argumentele de tip text trebuie să fie între ghilimele duble sau simple.

---

## 4. Operatori fără funcții

Pe lângă funcții, puteți folosi operatori direct în formulă. Sintaxa este similară cu Python.

### 4.1. Aritmetici

| Operație | Exemplu |
| -------- | ------- |
| Adunare | `=A1+A2+A3` |
| Scădere | `=A1-A2` |
| Înmulțire | `=A1*B1` |
| Împărțire | `=A1/B1` |
| Modulo | `=A1%B1` |
| Putere | `=A1**2` (sau `=A1^2`) |

### 4.2. Compărări

Returnează `True` sau `False` (afișate ca `Adevărat` / `Fals`).

| Operator | Semnificație | Exemplu |
| -------- | ------------ | ------- |
| `>` | Mai mare decât | `=A1>B1` |
| `<` | Mai mic decât | `=A1<B1` |
| `>=` | Mai mare sau egal | `=A1>=10` |
| `<=` | Mai mic sau egal | `=A1<=10` |
| `==` | Egal | `=A1==B1` |
| `!=` | Diferit | `=A1!=B1` |

### 4.3. Logici și condiționali

Puteți combina condițiile cu `and`, `or`, `not`.

```excel
= A1>5 and B1<10
= not(A1==0)
= 10 if A1>5 else 0
```

Operatorul ternar `if` `else` este de asemenea suportat direct.

---

## 5. Exemple practice

### 5.1. Suma vânzărilor

Presupunem că aveți vânzări în `B2:B10` și doriți totalul:

```excel
=SUM(B2:B10)
```

### 5.2. Discount condițional

Dacă totalul (în `B12`) depășește 100, aplicați 10% discount; altfel 0:

```excel
= IF(B12>100, B12*0.9, B12)
```

### 5.3. Medie și numărare

Media notelor în `C2:C20`, dar numai dacă există cel puțin 5 valori:

```excel
= IF(COUNT(C2:C20)>=5, AVG(C2:C20), "Date insuficiente")
```

### 5.4. Text combinat

Unirea numelui (A2) și prenumelui (B2) cu un spațiu:

```excel
= CONCAT(A2, " ", B2)
```

### 5.5. Rădăcina pătrată a unui număr

```excel
= SQRT(A1)
```

### 5.6. Rotunjire la 2 zecimale

```excel
= ROUND(A1, 2)
```

---

## 6. Sfaturi și trucuri

- **Referințe relative/absolut:** deocamdată, toate referințele sunt relative (ca în Excel). `$A$1` nu este încă suportat.
- **Intervale dinamice:** puteți folosi intervale precum `A:A` (întreaga coloană) sau `1:1` (întregul rând).
- **Autocompletare:** la scrierea `=`, apare un meniu cu funcțiile disponibile.
- **Erori:** dacă o formulă este invalidă, celula va afișa `#ERROR`, iar mesajul detaliat în bara de stare.
- **Recalculare:** formulele se actualizează automat la modificarea celulelor dependente.

---

## 8. Întrebări frecvente

**Cum se exportă datele?**
Deocamdată nu există export nativ, dar puteți accesa datele prin modelul intern.

**Suportă grafice?**
Nu, este o foaie de calcul de bază. Puteți combina cu alte widget-uri pentru vizualizare.

---

Bucurați-vă de utilizarea foii de calcul ușoare!

© 2026 — FloWorks
