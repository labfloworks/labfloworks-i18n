# 📊 Tutorial arkusza kalkulacyjnego

Lekki, osadzalny arkusz kalkulacyjny w stylu Excela.

---

## 1. Co to jest Spreadsheet?

To komponent arkusza kalkulacyjnego, który oferuje:

- Komórki z obsługą formuł (zaczynają się od `=`)
- Predefiniowane funkcje (SUMA, ŚREDNIA, WARUNKOWE, itd.)
- Operatory arytmetyczne, logiczne i porównania
- Minimalistyczny interfejs, idealny do integracji z aplikacjami Qt

---

## 2. Podstawowa nawigacja

- Kliknij komórkę, aby ją zaznaczyć.
- Pisz bezpośrednio, aby wprowadzić tekst lub liczby.
- Aby napisać **formułę**, zacznij od `=` (np. `=SUM(A1:A5)`).
- Naciśnij **Enter**, aby potwierdzić edycję.
- Użyj klawiszy strzałek lub myszy, aby się poruszać.

---

## 3. Formuły: dostępne funkcje

Wszystkie funkcje pisane są wielkimi literami i obsługują zakresy (np. `A1:A5`) lub argumenty rozdzielone przecinkami.

| Funkcja | Co robi | Przykład |
| ------- | ------- | -------- |
| `SUM` | Sumuje liczby | `=SUM(A1:A5)` |
| `AVG` / `AVERAGE` | Średnia | `=AVG(A1:A5)` |
| `COUNT` | Liczy liczby (niepuste) | `=COUNT(A1:A5)` |
| `MAX` | Wartość maksymalna | `=MAX(A1:A5)` |
| `MIN` | Wartość minimalna | `=MIN(A1:A5)` |
| `ABS` | Wartość bezwzględna | `=ABS(A1)` |
| `ROUND` | Zaokrągla (2. argument = miejsca dziesiętne) | `=ROUND(A1, 2)` |
| `IF` | Warunek (jeśli, to, w przeciwnym razie) | `=IF(A1>10, "Tak", "Nie")` |
| `CONCAT` / `CONCATENATE` | Łączy teksty | `=CONCAT(A1, " ", B1)` |
| `LEN` | Długość tekstu | `=LEN(A1)` |
| `INT` | Część całkowita | `=INT(A1)` |
| `SQRT` | Pierwiastek kwadratowy | `=SQRT(A1)` |

### 3.1. Uwagi dotyczące funkcji

- Zakresy wskazuje się za pomocą **dwukropka**: `A1:A5` obejmuje wszystkie komórki od A1 do A5.
- Funkcje można zagnieżdżać: `=SUM(A1:A5) + MAX(B1:B5)`.
- Argumenty tekstowe muszą być w cudzysłowie podwójnym lub pojedynczym.

---

## 4. Operatory bez funkcji

Oprócz funkcji możesz używać operatorów bezpośrednio w formule. Składnia jest podobna do Pythona.

### 4.1. Arytmetyczne

| Operacja | Przykład |
| -------- | -------- |
| Dodawanie | `=A1+A2+A3` |
| Odejmowanie | `=A1-A2` |
| Mnożenie | `=A1*B1` |
| Dzielenie | `=A1/B1` |
| Modulo | `=A1%B1` |
| Potęgowanie | `=A1**2` (lub `=A1^2`) |

### 4.2. Porównania

Zwracają `True` lub `False` (wyświetlane jako `Prawda` / `Fałsz`).

| Operator | Znaczenie | Przykład |
| -------- | --------- | -------- |
| `>` | Większe niż | `=A1>B1` |
| `<` | Mniejsze niż | `=A1<B1` |
| `>=` | Większe lub równe | `=A1>=10` |
| `<=` | Mniejsze lub równe | `=A1<=10` |
| `==` | Równe | `=A1==B1` |
| `!=` | Różne | `=A1!=B1` |

### 4.3. Logiczne i warunkowe

Możesz łączyć warunki za pomocą `and`, `or`, `not`.

```excel
= A1>5 and B1<10
= not(A1==0)
= 10 if A1>5 else 0
```

Operator trójargumentowy `if` `else` jest również obsługiwany bezpośrednio.

---

## 5. Praktyczne przykłady

### 5.1. Suma sprzedaży

Załóżmy, że masz sprzedaż w `B2:B10` i chcesz sumę:

```excel
=SUM(B2:B10)
```

### 5.2. Rabat warunkowy

Jeśli suma (w `B12`) przekracza 100, zastosuj 10% rabat; w przeciwnym razie 0:

```excel
= IF(B12>100, B12*0.9, B12)
```

### 5.3. Średnia i liczba

Średnia ocen w `C2:C20`, ale tylko jeśli jest co najmniej 5 wartości:

```excel
= IF(COUNT(C2:C20)>=5, AVG(C2:C20), "Niewystarczające dane")
```

### 5.4. Połączony tekst

Połączenie imienia (A2) i nazwiska (B2) ze spacją:

```excel
= CONCAT(A2, " ", B2)
```

### 5.5. Pierwiastek kwadratowy z liczby

```excel
= SQRT(A1)
```

### 5.6. Zaokrąglenie do 2 miejsc dziesiętnych

```excel
= ROUND(A1, 2)
```

---

## 6. Wskazówki i triki

- **Odwołania względne/bezwzględne:** na razie wszystkie odwołania są względne (jak w Excelu). `$A$1` nie jest jeszcze obsługiwane.
- **Zakresy dynamiczne:** możesz używać zakresów takich jak `A:A` (cała kolumna) lub `1:1` (cały wiersz).
- **Autouzupełnianie:** podczas pisania `=` pojawia się menu z dostępnymi funkcjami.
- **Błędy:** jeśli formuła jest nieprawidłowa, komórka wyświetli `#ERROR`, a szczegółowy komunikat na pasku stanu.
- **Przeliczanie:** formuły aktualizują się automatycznie po modyfikacji zależnych komórek.

---

## 8. Często zadawane pytania

**Jak wyeksportować dane?**
Na razie nie ma natywnego eksportu, ale możesz uzyskać dostęp do danych za pomocą wewnętrznego modelu.

**Czy obsługuje wykresy?**
Nie, to podstawowy arkusz kalkulacyjny. Możesz go połączyć z innymi widżetami do wizualizacji.

---

Ciesz się używaniem lekkiego arkusza kalkulacyjnego!

© 2026 — FloWorks
