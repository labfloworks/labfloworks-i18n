# 📊 Spreadsheet-Tutorial

Leichte, einbettbare Basic-Tabellenkalkulation im Excel-Stil.  

---

## 1. Was ist Spreadsheet?

Es ist eine Tabellenkalkulationskomponente, die bietet:

- Zellen mit Formelunterstützung (beginnen mit `=`)
- Vordefinierte Funktionen (SUM, AVG, BEDINGTE usw.)
- Arithmetische, logische und Vergleichsoperatoren
- Minimalistische Benutzeroberfläche, ideal für die Integration in Qt-Anwendungen

---

## 2. Grundlegende Navigation

- Klicke auf eine Zelle, um sie auszuwählen.
- Schreibe direkt, um Text oder Zahlen einzugeben.
- Um eine **Formel** zu schreiben, beginne mit `=` (z. B. `=SUM(A1:A5)`).
- Drücke **Enter**, um die Bearbeitung zu bestätigen.
- Verwende die Pfeiltasten oder die Maus, um dich zu bewegen.

---

## 3. Formeln: verfügbare Funktionen

Alle Funktionen werden in Großbuchstaben geschrieben und unterstützen Bereiche (z. B. `A1:A5`) oder durch Kommas getrennte Argumente.

| Funktion                  | Was sie tut                          | Beispiel                        |
| ------------------------ | --------------------------------- | ------------------------------ |
| `SUM`                    | Zahlen summieren                      | `=SUM(A1:A5)`                  |
| `AVG` / `AVERAGE`        | Durchschnitt                          | `=AVG(A1:A5)`                  |
| `COUNT`                  | Zahlen zählen (nicht leer)        | `=COUNT(A1:A5)`                |
| `MAX`                    | Maximaler Wert                      | `=MAX(A1:A5)`                  |
| `MIN`                    | Minimaler Wert                      | `=MIN(A1:A5)`                  |
| `ABS`                    | Absoluter Wert                    | `=ABS(A1)`                     |
| `ROUND`                  | Runden (2. Argument = Dezimalstellen) | `=ROUND(A1, 2)`               |
| `IF`                     | Bedingt (wenn, dann, sonst) | `=IF(A1>10, "Ja", "Nein")`       |
| `CONCAT` / `CONCATENATE` | Texte verbinden                        | `=CONCAT(A1, " ", B1)`         |
| `LEN`                    | Textlänge                 | `=LEN(A1)`                     |
| `INT`                    | Ganzzahliger Teil                      | `=INT(A1)`                     |
| `SQRT`                   | Quadratwurzel                     | `=SQRT(A1)`                    |

### 3.1. Hinweise zu Funktionen

- Bereiche werden mit **Doppelpunkt** angegeben: `A1:A5` umfasst alle Zellen von A1 bis A5.
- Funktionen können verschachtelt werden: `=SUM(A1:A5) + MAX(B1:B5)`.
- Textargumente müssen in doppelte oder einfache Anführungszeichen gesetzt werden.

---

## 4. Operatoren ohne Funktionen

Zusätzlich zu Funktionen kannst du Operatoren direkt in der Formel verwenden. Die Syntax ist ähnlich wie in Python.

### 4.1. Arithmetisch

| Operation      | Beispiel                   |
| -------------- | ------------------------- |
| Addition           | `=A1+A2+A3`               |
| Subtraktion          | `=A1-A2`                  |
| Multiplikation | `=A1*B1`                  |
| Division       | `=A1/B1`                  |
| Modulo         | `=A1%B1`                  |
| Potenz       | `=A1**2`  (oder `=A1^2`)     |

### 4.2. Vergleiche

Geben `True` oder `False` zurück (angezeigt als `Wahr` / `Falsch`).

| Operator | Bedeutung       | Beispiel            |
| -------- | ----------------- | ------------------ |
| `>`      | Größer als         | `=A1>B1`           |
| `<`      | Kleiner als         | `=A1<B1`           |
| `>=`     | Größer oder gleich     | `=A1>=10`          |
| `<=`     | Kleiner oder gleich     | `=A1<=10`          |
| `==`     | Gleich             | `=A1==B1`          |
| `!=`     | Ungleich          | `=A1!=B1`          |

### 4.3. Logisch und bedingt

Du kannst Bedingungen mit `and`, `or`, `not` kombinieren.

```excel
= A1>5 and B1<10
= not(A1==0)
= 10 if A1>5 else 0
```

Der ternäre Operator `if` `else` wird auch direkt unterstützt.

---

## 5. Praktische Beispiele

### 5.1. Summe der Verkäufe

Angenommen, du hast Verkäufe in `B2:B10` und möchtest die Summe:

```excel
=SUM(B2:B10)
```

### 5.2. Bedingter Rabatt

Wenn die Summe (in `B12`) 100 übersteigt, wende 10 % Rabatt an; sonst 0:

```excel
= IF(B12>100, B12*0.9, B12)
```

### 5.3. Durchschnitt und Anzahl

Durchschnitt der Noten in `C2:C20`, aber nur wenn mindestens 5 Werte vorhanden sind:

```excel
= IF(COUNT(C2:C20)>=5, AVG(C2:C20), "Unzureichende Daten")
```

### 5.4. Kombinierter Text

Vorname (A2) und Nachname (B2) mit einem Leerzeichen verbinden:

```excel
= CONCAT(A2, " ", B2)
```

### 5.5. Quadratwurzel einer Zahl

```excel
= SQRT(A1)
```

### 5.6. Rundung auf 2 Dezimalstellen

```excel
= ROUND(A1, 2)
```

---

## 6. Tipps und Tricks

- **Relative/absolute Bezüge:** vorerst sind alle Bezüge relativ (wie in Excel). `$A$1` wird noch nicht unterstützt.
- **Dynamische Bereiche:** du kannst Bereiche wie `A:A` (gesamte Spalte) oder `1:1` (gesamte Zeile) verwenden.
- **Autovervollständigung:** beim Schreiben von `=` erscheint ein Menü mit den verfügbaren Funktionen.
- **Fehler:** wenn eine Formel ungültig ist, zeigt die Zelle `#ERROR` und die detaillierte Meldung in der Statusleiste an.
- **Neuberechnung:** Formeln werden automatisch aktualisiert, wenn abhängige Zellen geändert werden.

---

## 8. Häufig gestellte Fragen

**Wie exportiere ich Daten?**  
Derzeit gibt es keinen nativen Export, aber du kannst über das interne Modell auf die Daten zugreifen.

**Werden Diagramme unterstützt?**  
Nein, es ist eine Basic-Tabellenkalkulation. Du kannst sie mit anderen Widgets für die Visualisierung kombinieren.

---

Viel Spaß mit der leichten Tabellenkalkulation!

© 2026 — FloWorks
