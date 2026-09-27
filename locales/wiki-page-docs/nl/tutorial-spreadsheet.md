# 📊 Spreadsheet-tutorial

Lichtgewicht, inbouwbaar spreadsheet in Excel-stijl.

---

## 1. Wat is Spreadsheet?

Het is een spreadsheetcomponent die biedt:

- Cellen met formuleondersteuning (beginnen met `=`)
- Vooraf gedefinieerde functies (SUM, AVERAGE, CONDITIONEEL, etc.)
- Rekenkundige, logische en vergelijkingsoperatoren
- Minimalistische interface, ideaal voor integratie in Qt-applicaties

---

## 2. Basisnavigatie

- Klik op een cel om deze te selecteren.
- Typ direct om tekst of getallen in te voeren.
- Om een **formule** te schrijven, begin met `=` (bijv. `=SUM(A1:A5)`).
- Druk op **Enter** om de bewerking te bevestigen.
- Gebruik de pijltjestoetsen of de muis om te navigeren.

---

## 3. Formules: beschikbare functies

Alle functies worden in hoofdletters geschreven en ondersteunen bereiken (bijv. `A1:A5`) of argumenten gescheiden door een komma.

| Functie | Wat doet het | Voorbeeld |
| ------- | ------------ | --------- |
| `SUM` | Telt getallen op | `=SUM(A1:A5)` |
| `AVG` / `AVERAGE` | Gemiddelde | `=AVG(A1:A5)` |
| `COUNT` | Telt getallen (niet leeg) | `=COUNT(A1:A5)` |
| `MAX` | Maximale waarde | `=MAX(A1:A5)` |
| `MIN` | Minimale waarde | `=MIN(A1:A5)` |
| `ABS` | Absolute waarde | `=ABS(A1)` |
| `ROUND` | Afronding (2e argument = decimalen) | `=ROUND(A1, 2)` |
| `IF` | Conditioneel (als, dan, anders) | `=IF(A1>10, "Ja", "Nee")` |
| `CONCAT` / `CONCATENATE` | Tekst samenvoegen | `=CONCAT(A1, " ", B1)` |
| `LEN` | Tekstlengte | `=LEN(A1)` |
| `INT` | Geheel getal | `=INT(A1)` |
| `SQRT` | Vierkantswortel | `=SQRT(A1)` |

### 3.1. Opmerkingen over functies

- Bereiken worden aangegeven met een **dubbele punt**: `A1:A5` omvat alle cellen van A1 tot A5.
- Functies kunnen genest worden: `=SUM(A1:A5) + MAX(B1:B5)`.
- Tekstargumenten moeten tussen dubbele of enkele aanhalingstekens staan.

---

## 4. Operatoren zonder functies

Naast functies kunt u operatoren direct in de formule gebruiken. De syntaxis is vergelijkbaar met Python.

### 4.1. Rekenkundig

| Bewerking | Voorbeeld |
| --------- | --------- |
| Optellen | `=A1+A2+A3` |
| Aftrekken | `=A1-A2` |
| Vermenigvuldigen | `=A1*B1` |
| Delen | `=A1/B1` |
| Modulo | `=A1%B1` |
| Macht | `=A1**2` (of `=A1^2`) |

### 4.2. Vergelijkingen

Retourneren `True` of `False` (weergegeven als `Waar` / `Onwaar`).

| Operator | Betekenis | Voorbeeld |
| -------- | --------- | --------- |
| `>` | Groter dan | `=A1>B1` |
| `<` | Kleiner dan | `=A1<B1` |
| `>=` | Groter of gelijk | `=A1>=10` |
| `<=` | Kleiner of gelijk | `=A1<=10` |
| `==` | Gelijk | `=A1==B1` |
| `!=` | Niet gelijk | `=A1!=B1` |

### 4.3. Logisch en conditioneel

U kunt voorwaarden combineren met `and`, `or`, `not`.

```excel
= A1>5 and B1<10
= not(A1==0)
= 10 if A1>5 else 0
```

De ternaire operator `if` `else` wordt ook direct ondersteund.

---

## 5. Praktische voorbeelden

### 5.1. Som van verkopen

Stel dat u verkopen heeft in `B2:B10` en het totaal wilt:

```excel
=SUM(B2:B10)
```

### 5.2. Voorwaardelijke korting

Als het totaal (in `B12`) hoger is dan 100, pas dan 10% korting toe; anders 0:

```excel
= IF(B12>100, B12*0.9, B12)
```

### 5.3. Gemiddelde en aantal

Gemiddelde van cijfers in `C2:C20`, maar alleen als er minstens 5 waarden zijn:

```excel
= IF(COUNT(C2:C20)>=5, AVG(C2:C20), "Onvoldoende gegevens")
```

### 5.4. Gecombineerde tekst

Voornaam (A2) en achternaam (B2) samenvoegen met een spatie:

```excel
= CONCAT(A2, " ", B2)
```

### 5.5. Vierkantswortel van een getal

```excel
= SQRT(A1)
```

### 5.6. Afronden op 2 decimalen

```excel
= ROUND(A1, 2)
```

---

## 6. Tips en trucs

- **Relatieve/absoluut verwijzingen:** voor nu zijn alle verwijzingen relatief (zoals in Excel). `$A$1` wordt nog niet ondersteund.
- **Dynamische bereiken:** u kunt bereiken gebruiken zoals `A:A` (hele kolom) of `1:1` (hele rij).
- **Autocompletie:** bij het typen van `=` verschijnt een menu met beschikbare functies.
- **Fouten:** als een formule ongeldig is, toont de cel `#ERROR` en het gedetailleerde bericht in de statusbalk.
- **Herberekening:** formules worden automatisch bijgewerkt bij wijziging van afhankelijke cellen.

---

## 8. Veelgestelde vragen

**Hoe exporteer ik gegevens?**
Er is nog geen native export, maar u kunt toegang krijgen tot de gegevens via het interne model.

**Ondersteunt het grafieken?**
Nee, het is een basis spreadsheet. U kunt het combineren met andere widgets voor visualisatie.

---

Geniet van het gebruik van de lichtgewicht spreadsheet!

© 2026 — FloWorks
