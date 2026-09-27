# 📊 Spreadsheet Tutorial

Lightweight, Excel-style spreadsheet, embeddable.

---

## 1. What is Spreadsheet?

It is a spreadsheet component that offers:

- Cells with formula support (start with `=`)
- Predefined functions (SUM, AVERAGE, CONDITIONALS, etc.)
- Arithmetic, logical and comparison operators
- Minimalist interface, ideal for integrating into Qt applications

---

## 2. Basic navigation

- Click on a cell to select it.
- Type directly to enter text or numbers.
- To write a **formula**, start with `=` (e.g. `=SUM(A1:A5)`).
- Press **Enter** to confirm the edit.
- Use the arrow keys or the mouse to move around.

---

## 3. Formulas: available functions

All functions are written in uppercase and support ranges (e.g. `A1:A5`) or arguments separated by commas.

| Function                 | What it does                     | Example                        |
| ------------------------ | -------------------------------- | ------------------------------ |
| `SUM`                    | Sums numbers                     | `=SUM(A1:A5)`                  |
| `AVG` / `AVERAGE`        | Average                          | `=AVG(A1:A5)`                  |
| `COUNT`                  | Counts numbers (non-empty)       | `=COUNT(A1:A5)`                |
| `MAX`                    | Maximum value                    | `=MAX(A1:A5)`                  |
| `MIN`                    | Minimum value                    | `=MIN(A1:A5)`                  |
| `ABS`                    | Absolute value                   | `=ABS(A1)`                     |
| `ROUND`                  | Rounds (2nd argument = decimals) | `=ROUND(A1, 2)`               |
| `IF`                     | Conditional (if, then, else)     | `=IF(A1>10, "Yes", "No")`      |
| `CONCAT` / `CONCATENATE` | Joins text                       | `=CONCAT(A1, " ", B1)`         |
| `LEN`                    | Text length                      | `=LEN(A1)`                     |
| `INT`                    | Integer part                     | `=INT(A1)`                     |
| `SQRT`                   | Square root                      | `=SQRT(A1)`                    |

### 3.1. Notes on functions

- Ranges are indicated with **colons**: `A1:A5` includes all cells from A1 to A5.
- Functions can be nested: `=SUM(A1:A5) + MAX(B1:B5)`.
- Text arguments must be enclosed in double or single quotes.

---

## 4. Operators without functions

In addition to functions, you can use operators directly in the formula. The syntax is similar to Python.

### 4.1. Arithmetic

| Operation      | Example                   |
| -------------- | ------------------------- |
| Addition       | `=A1+A2+A3`               |
| Subtraction    | `=A1-A2`                  |
| Multiplication | `=A1*B1`                  |
| Division       | `=A1/B1`                  |
| Modulo         | `=A1%B1`                  |
| Power          | `=A1**2`  (or `=A1^2`)   |

### 4.2. Comparisons

They return `True` or `False` (displayed as `True` / `False`).

| Operator | Meaning           | Example            |
| -------- | ----------------- | ------------------ |
| `>`      | Greater than      | `=A1>B1`           |
| `<`      | Less than         | `=A1<B1`           |
| `>=`     | Greater or equal  | `=A1>=10`          |
| `<=`     | Less or equal     | `=A1<=10`          |
| `==`     | Equal             | `=A1==B1`          |
| `!=`     | Not equal         | `=A1!=B1`          |

### 4.3. Logical and conditional

You can combine conditions with `and`, `or`, `not`.

```excel
= A1>5 and B1<10
= not(A1==0)
= 10 if A1>5 else 0
```

The ternary `if` `else` operator is also supported directly.

---

## 5. Practical examples

### 5.1. Sum of sales

Suppose you have sales in `B2:B10` and you want the total:

```excel
=SUM(B2:B10)
```

### 5.2. Conditional discount

If the total (in `B12`) exceeds 100, apply a 10% discount; otherwise, 0:

```excel
= IF(B12>100, B12*0.9, B12)
```

### 5.3. Average and count

Average of grades in `C2:C20`, but only if there are at least 5 values:

```excel
= IF(COUNT(C2:C20)>=5, AVG(C2:C20), "Insufficient data")
```

### 5.4. Combined text

Join first name (A2) and last name (B2) with a space:

```excel
= CONCAT(A2, " ", B2)
```

### 5.5. Square root of a number

```excel
= SQRT(A1)
```

### 5.6. Rounding to 2 decimals

```excel
= ROUND(A1, 2)
```

---

## 6. Tips and tricks

- **Relative/absolute references:** for now, all references are relative (like Excel). `$A$1` is not supported yet.
- **Dynamic ranges:** you can use ranges like `A:A` (whole column) or `1:1` (whole row).
- **Autocompletion:** when typing `=`, a menu with available functions appears.
- **Errors:** if a formula is invalid, the cell will show `#ERROR` and the detailed message in the status bar.
- **Recalculation:** formulas update automatically when dependent cells are modified.

---

## 8. Frequently asked questions

**How to export data?**
For now there is no native export, but you can access the data through the internal model.

**Does it support charts?**
No, it is a basic spreadsheet. You can combine it with other widgets for visualization.

---

Enjoy using the lightweight spreadsheet!

© 2026 — FloWorks
