# 📊 Tutoriál pro Spreadsheet

Základní tabulkový kalkulátor ve stylu Excelu, lehký a embedovatelný.

---

## 1. Co je Spreadsheet?

Je komponenta tabulkového kalkulátoru, která nabízí:

- Buňky s podporou vzorců (začínají `=`)
- Předdefinované funkce (SUMA, PRŮMĚR, PODMÍNKOVÉ, atd.)
- Aritmetické, logické a porovnávací operátory
- Minimalistické rozhraní, ideální pro integraci do aplikací Qt

---

## 2. Základní navigace

- Kliknutím na buňku ji vyberete.
- Pište přímo pro zadání textu nebo čísel.
- Pro napsání **vzorce** začněte `=` (např. `=SUM(A1:A5)`).
- Stiskněte **Enter** pro potvrzení úpravy.
- Použijte šipky nebo myš k pohybu.

---

## 3. Vzorce: dostupné funkce

Všechny funkce se píší velkými písmeny a podporují rozsahy (např. `A1:A5`) nebo argumenty oddělené čárkou.

| Funkce | Co dělá | Příklad |
| ------ | ------- | ------- |
| `SUM` | Sečte čísla | `=SUM(A1:A5)` |
| `AVG` / `AVERAGE` | Průměr | `=AVG(A1:A5)` |
| `COUNT` | Spočítá čísla (ne prázdné) | `=COUNT(A1:A5)` |
| `MAX` | Maximální hodnota | `=MAX(A1:A5)` |
| `MIN` | Minimální hodnota | `=MIN(A1:A5)` |
| `ABS` | Absolutní hodnota | `=ABS(A1)` |
| `ROUND` | Zaokrouhlí (2. argument = desetinná místa) | `=ROUND(A1, 2)` |
| `IF` | Podmínka (pokud, pak, jinak) | `=IF(A1>10, "Ano", "Ne")` |
| `CONCAT` / `CONCATENATE` | Spojí texty | `=CONCAT(A1, " ", B1)` |
| `LEN` | Délka textu | `=LEN(A1)` |
| `INT` | Celá část | `=INT(A1)` |
| `SQRT` | Druhá odmocnina | `=SQRT(A1)` |

### 3.1. Poznámky k funkcím

- Rozsahy se uvádějí pomocí **dvojtečky**: `A1:A5` zahrnuje všechny buňky od A1 do A5.
- Funkce lze vnořovat: `=SUM(A1:A5) + MAX(B1:B5)`.
- Textové argumenty musí být v uvozovkách dvojitých nebo jednoduchých.

---

## 4. Operátory bez funkcí

Kromě funkcí můžete použít operátory přímo ve vzorci. Syntaxe je podobná Pythonu.

### 4.1. Aritmetické

| Operace | Příklad |
| ------- | ------- |
| Sčítání | `=A1+A2+A3` |
| Odčítání | `=A1-A2` |
| Násobení | `=A1*B1` |
| Dělení | `=A1/B1` |
| Modulo | `=A1%B1` |
| Mocnina | `=A1**2` (nebo `=A1^2`) |

### 4.2. Porovnání

Vrací `True` nebo `False` (zobrazeno jako `Pravda` / `Nepravda`).

| Operátor | Význam | Příklad |
| -------- | ------ | ------- |
| `>` | Větší než | `=A1>B1` |
| `<` | Menší než | `=A1<B1` |
| `>=` | Větší nebo rovno | `=A1>=10` |
| `<=` | Menší nebo rovno | `=A1<=10` |
| `==` | Rovno | `=A1==B1` |
| `!=` | Různé od | `=A1!=B1` |

### 4.3. Logické a podmíněné

Můžete kombinovat podmínky pomocí `and`, `or`, `not`.

```excel
= A1>5 and B1<10
= not(A1==0)
= 10 if A1>5 else 0
```

Ternární operátor `if` `else` je také přímo podporován.

---

## 5. Praktické příklady

### 5.1. Součet prodejů

Předpokládejme, že máte prodeje v `B2:B10` a chcete celkovou sumu:

```excel
=SUM(B2:B10)
```

### 5.2. Podmíněná sleva

Pokud celková částka (v `B12`) překročí 100, uplatní se sleva 10 %; jinak 0:

```excel
= IF(B12>100, B12*0.9, B12)
```

### 5.3. Průměr a počet

Průměr známek v `C2:C20`, ale pouze pokud je alespoň 5 hodnot:

```excel
= IF(COUNT(C2:C20)>=5, AVG(C2:C20), "Nedostatek dat")
```

### 5.4. Kombinovaný text

Spojení jména (A2) a příjmení (B2) s mezerou:

```excel
= CONCAT(A2, " ", B2)
```

### 5.5. Druhá odmocnina čísla

```excel
= SQRT(A1)
```

### 5.6. Zaokrouhlení na 2 desetinná místa

```excel
= ROUND(A1, 2)
```

---

## 6. Tipy a triky

- **Relativní/absolutní reference:** prozatím jsou všechny reference relativní (jako v Excelu). `$A$1` zatím není podporováno.
- **Dynamické rozsahy:** můžete použít rozsahy jako `A:A` (celý sloupec) nebo `1:1` (celý řádek).
- **Autodoplňování:** při psaní `=` se zobrazí nabídka dostupných funkcí.
- **Chyby:** pokud je vzorec neplatný, buňka zobrazí `#ERROR` a podrobnou zprávu ve stavovém řádku.
- **Přepočet:** vzorce se automaticky aktualizují při změně závislých buněk.

---

## 8. Časté dotazy

**Jak exportovat data?**
Zatím není k dispozici nativní export, ale můžete přistupovat k datům prostřednictvím interního modelu.

**Podporuje grafy?**
Ne, je to základní tabulkový kalkulátor. Pro vizualizaci můžete kombinovat s jinými widgety.

---

Užijte si používání lehkého tabulkového kalkulátoru!

© 2026 — FloWorks
