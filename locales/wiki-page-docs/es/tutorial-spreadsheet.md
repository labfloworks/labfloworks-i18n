# 📊 Tutorial de Spreadsheet

Hoja de cálculo básica estilo Excel, ligera y embebible.  

---

## 1. ¿Qué es Spreadsheet?

Es un componente de hoja de cálculo que ofrece:

- Celdas con soporte para fórmulas (empiezan con `=`)
- Funciones predefinidas (SUMA, PROMEDIO, CONDICIONALES, etc.)
- Operadores aritméticos, lógicos y de comparación
- Interfaz minimalista, ideal para integrar en aplicaciones Qt


---

## 2. Navegación básica

- Haz clic en una celda para seleccionarla.
- Escribe directamente para introducir texto o números.
- Para escribir una **fórmula**, comienza con `=` (ej. `=SUM(A1:A5)`).
- Presiona **Enter** para confirmar la edición.
- Usa las teclas de flecha o el ratón para moverte.

---

## 3. Fórmulas: funciones disponibles

Todas las funciones se escriben en mayúsculas y admiten rangos (ej. `A1:A5`) o argumentos separados por coma.

| Función                  | Qué hace                          | Ejemplo                        |
| ------------------------ | --------------------------------- | ------------------------------ |
| `SUM`                    | Suma números                      | `=SUM(A1:A5)`                  |
| `AVG` / `AVERAGE`        | Promedio                          | `=AVG(A1:A5)`                  |
| `COUNT`                  | Cuenta números (no vacíos)        | `=COUNT(A1:A5)`                |
| `MAX`                    | Valor máximo                      | `=MAX(A1:A5)`                  |
| `MIN`                    | Valor mínimo                      | `=MIN(A1:A5)`                  |
| `ABS`                    | Valor absoluto                    | `=ABS(A1)`                     |
| `ROUND`                  | Redondea (2º argumento = decimales) | `=ROUND(A1, 2)`               |
| `IF`                     | Condicional (si, entonces, si_no) | `=IF(A1>10, "Sí", "No")`       |
| `CONCAT` / `CONCATENATE` | Une textos                        | `=CONCAT(A1, " ", B1)`         |
| `LEN`                    | Longitud de texto                 | `=LEN(A1)`                     |
| `INT`                    | Parte entera                      | `=INT(A1)`                     |
| `SQRT`                   | Raíz cuadrada                     | `=SQRT(A1)`                    |

### 3.1. Notas sobre las funciones

- Los rangos se indican con **dos puntos**: `A1:A5` incluye todas las celdas desde A1 hasta A5.
- Las funciones pueden anidarse: `=SUM(A1:A5) + MAX(B1:B5)`.
- Los argumentos de texto deben ir entre comillas dobles o simples.

---

## 4. Operadores sin funciones

Además de las funciones, puedes usar operadores directamente en la fórmula. La sintaxis es similar a Python.

### 4.1. Aritméticos

| Operación      | Ejemplo                   |
| -------------- | ------------------------- |
| Suma           | `=A1+A2+A3`               |
| Resta          | `=A1-A2`                  |
| Multiplicación | `=A1*B1`                  |
| División       | `=A1/B1`                  |
| Módulo         | `=A1%B1`                  |
| Potencia       | `=A1**2`  (o `=A1^2`)     |

### 4.2. Comparaciones

Devuelven `True` o `False` (que se muestran como `Verdadero` / `Falso`).

| Operador | Significado       | Ejemplo            |
| -------- | ----------------- | ------------------ |
| `>`      | Mayor que         | `=A1>B1`           |
| `<`      | Menor que         | `=A1<B1`           |
| `>=`     | Mayor o igual     | `=A1>=10`          |
| `<=`     | Menor o igual     | `=A1<=10`          |
| `==`     | Igual             | `=A1==B1`          |
| `!=`     | Distinto          | `=A1!=B1`          |

### 4.3. Lógicos y condicionales

Puedes combinar condiciones con `and`, `or`, `not`.

```excel
= A1>5 and B1<10
= not(A1==0)
= 10 if A1>5 else 0
```

El operador ternario `if` `else` también está soportado directamente.

---

## 5. Ejemplos prácticos

### 5.1. Suma de ventas

Supón que tienes ventas en `B2:B10` y quieres el total:

```excel
=SUM(B2:B10)
```

### 5.2. Descuento condicional

Si el total (en `B12`) supera 100, aplica un 10% de descuento; si no, 0:

```excel
= IF(B12>100, B12*0.9, B12)
```

### 5.3. Promedio y contar

Promedio de notas en `C2:C20`, pero solo si hay al menos 5 valores:

```excel
= IF(COUNT(C2:C20)>=5, AVG(C2:C20), "Insuficientes datos")
```

### 5.4. Texto combinado

Unir nombre (A2) y apellido (B2) con un espacio:

```excel
= CONCAT(A2, " ", B2)
```

### 5.5. Raíz cuadrada de un número

```excel
= SQRT(A1)
```

### 5.6. Redondeo a 2 decimales

```excel
= ROUND(A1, 2)
```

---

## 6. Consejos y trucos

- **Referencias relativas/absolutas:** por ahora, todas las referencias son relativas (como Excel). No se soporta `$A$1` aún.
- **Rangos dinámicos:** puedes usar rangos como `A:A` (toda la columna) o `1:1` (toda la fila).
- **Autocompletado:** al escribir `=`, aparece un menú con las funciones disponibles.
- **Errores:** si una fórmula es inválida, la celda mostrará `#ERROR` y el mensaje detallado en la barra de estado.
- **Recálculo:** las fórmulas se actualizan automáticamente al modificar celdas dependientes.

---

## 8. Preguntas frecuentes

**¿Cómo exportar datos?**  
Por ahora no hay exportación nativa, pero puedes acceder a los datos mediante el modelo interno.

**¿Soporta gráficos?**  
No, es una hoja de cálculo básica. Puedes combinar con otros widgets para visualización.

---

¡Disfruta el uso de la hoja de cálculo ligera!

© 2026 — FloWorks
