# 📊 Tutorial de Spreadsheet

Folha de cálculo básica estilo Excel, leve e incorporável.  

---

## 1. O que é Spreadsheet?

É um componente de folha de cálculo que oferece:

- Células com suporte para fórmulas (começam com `=`)
- Funções predefinidas (SUM, AVG, CONDICIONAIS, etc.)
- Operadores aritméticos, lógicos e de comparação
- Interface minimalista, ideal para integrar em aplicações Qt

---

## 2. Navegação básica

- Clica numa célula para a selecionar.
- Escreve diretamente para introduzir texto ou números.
- Para escrever uma **fórmula**, começa com `=` (ex. `=SUM(A1:A5)`).
- Pressiona **Enter** para confirmar a edição.
- Usa as teclas de seta ou o rato para te moveres.

---

## 3. Fórmulas: funções disponíveis

Todas as funções são escritas em maiúsculas e admitem intervalos (ex. `A1:A5`) ou argumentos separados por vírgula.

| Função                  | O que faz                          | Exemplo                        |
| ------------------------ | --------------------------------- | ------------------------------ |
| `SUM`                    | Soma números                      | `=SUM(A1:A5)`                  |
| `AVG` / `AVERAGE`        | Média                          | `=AVG(A1:A5)`                  |
| `COUNT`                  | Conta números (não vazios)        | `=COUNT(A1:A5)`                |
| `MAX`                    | Valor máximo                      | `=MAX(A1:A5)`                  |
| `MIN`                    | Valor mínimo                      | `=MIN(A1:A5)`                  |
| `ABS`                    | Valor absoluto                    | `=ABS(A1)`                     |
| `ROUND`                  | Arredonda (2º argumento = casas decimais) | `=ROUND(A1, 2)`               |
| `IF`                     | Condicional (se, então, senão) | `=IF(A1>10, "Sim", "Não")`       |
| `CONCAT` / `CONCATENATE` | Une textos                        | `=CONCAT(A1, " ", B1)`         |
| `LEN`                    | Comprimento do texto                 | `=LEN(A1)`                     |
| `INT`                    | Parte inteira                      | `=INT(A1)`                     |
| `SQRT`                   | Raiz quadrada                     | `=SQRT(A1)`                    |

### 3.1. Notas sobre as funções

- Os intervalos indicam-se com **dois pontos**: `A1:A5` inclui todas as células desde A1 até A5.
- As funções podem ser aninhadas: `=SUM(A1:A5) + MAX(B1:B5)`.
- Os argumentos de texto devem ir entre aspas duplas ou simples.

---

## 4. Operadores sem funções

Além das funções, podes usar operadores diretamente na fórmula. A sintaxe é semelhante à de Python.

### 4.1. Aritméticos

| Operação      | Exemplo                   |
| -------------- | ------------------------- |
| Soma           | `=A1+A2+A3`               |
| Subtração          | `=A1-A2`                  |
| Multiplicação | `=A1*B1`                  |
| Divisão       | `=A1/B1`                  |
| Módulo         | `=A1%B1`                  |
| Potência       | `=A1**2`  (ou `=A1^2`)     |

### 4.2. Comparações

Devolvem `True` ou `False` (que se mostram como `Verdadeiro` / `Falso`).

| Operador | Significado       | Exemplo            |
| -------- | ----------------- | ------------------ |
| `>`      | Maior que         | `=A1>B1`           |
| `<`      | Menor que         | `=A1<B1`           |
| `>=`     | Maior ou igual     | `=A1>=10`          |
| `<=`     | Menor ou igual     | `=A1<=10`          |
| `==`     | Igual             | `=A1==B1`          |
| `!=`     | Diferente          | `=A1!=B1`          |

### 4.3. Lógicos e condicionais

Podes combinar condições com `and`, `or`, `not`.

```excel
= A1>5 and B1<10
= not(A1==0)
= 10 if A1>5 else 0
```

O operador ternário `if` `else` também está suportado diretamente.

---

## 5. Exemplos práticos

### 5.1. Soma de vendas

Supõe que tens vendas em `B2:B10` e queres o total:

```excel
=SUM(B2:B10)
```

### 5.2. Desconto condicional

Se o total (em `B12`) superar 100, aplica um desconto de 10%; senão, 0:

```excel
= IF(B12>100, B12*0.9, B12)
```

### 5.3. Média e contar

Média de notas em `C2:C20`, mas só se houver pelo menos 5 valores:

```excel
= IF(COUNT(C2:C20)>=5, AVG(C2:C20), "Dados insuficientes")
```

### 5.4. Texto combinado

Unir nome (A2) e apelido (B2) com um espaço:

```excel
= CONCAT(A2, " ", B2)
```

### 5.5. Raiz quadrada de um número

```excel
= SQRT(A1)
```

### 5.6. Arredondamento a 2 casas decimais

```excel
= ROUND(A1, 2)
```

---

## 6. Conselhos e truques

- **Referências relativas/absolutas:** por agora, todas as referências são relativas (como no Excel). `$A$1` ainda não é suportado.
- **Intervalos dinâmicos:** podes usar intervalos como `A:A` (toda a coluna) ou `1:1` (toda a linha).
- **Autocompletado:** ao escrever `=`, aparece um menu com as funções disponíveis.
- **Erros:** se uma fórmula for inválida, a célula mostrará `#ERROR` e a mensagem detalhada na barra de estado.
- **Recálculo:** as fórmulas atualizam-se automaticamente ao modificar células dependentes.

---

## 8. Perguntas frequentes

**Como exportar dados?**  
Por agora não há exportação nativa, mas podes aceder aos dados através do modelo interno.

**Suporta gráficos?**  
Não, é uma folha de cálculo básica. Podes combinar com outros widgets para visualização.

---

Aproveita o uso da folha de cálculo leve!

© 2026 — FloWorks
