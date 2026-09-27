# 🧪 Tutorial Interativo da Consola FloWorks

Bem-vindo ao laboratório de experimentação do FloWorks. Esta é uma secção para utilizadores mais avançados: trata-se de um terminal **Python** para controlar tudo o que está associado ao canvas, ou seja, nós e as suas conexões, de forma sequencial e por linhas de código; é um terminal ligado ao programa que o pode controlar e determinar comportamentos ou rotinas para a facilidade de utilizadores mais exigentes.

Este guia mostrar-te-á **passo a passo** como controlar e analisar os teus diagramas de fluxo sem tocar no rato. Cada exemplo foi validado na consola interativa e reflete a estrutura real de dados do programa.

---

## 1. Conhecer o terreno

A consola injeta três objetos globais: `app` (janela principal), `graph` (cenário/diagrama) e `selected_node` (nó atualmente selecionado no canvas). Todos os comandos partem destes três.

### Ver todos os nós

```python
>>> graph.nodes
```

**Exemplo de saída:**
```
Nós na cena:
  [0] Gerador de Sinais Avançado (tipo: SignalSourceNode, categoria: Sources)
  [1] FFT (tipo: FFTNode, categoria: Processing)
```

O índice entre parênteses retos (`[0]`, `[1]`) é a tua forma principal de aceder a um nó. A ordem é a de criação no canvas.

#### Alternativa: contar nós ou filtrar por tipo

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### Ver todas as conexões

```python
>>> graph.connections
```

**Exemplo de saída:**
```
Conexões na cena:
  [0] Gerador de Sinais Avançado (out) → FFT (input)
```

A saída mostra o nome do nó origem, o porto de saída, a seta, o nó destino e o porto de entrada. Se uma conexão não aparecer, o fluxo não poderá ser executado.

#### Alternativa: ver conexões de um único nó

```python
>>> selected_node.connectors
```

### Ver o nó selecionado

Clica num nó do canvas e depois executa:

```python
>>> selected_node
```

**Exemplo de saída:**
```
Nó: FFT
  Tipo: FFTNode
  Categoria: Processing
  Portos: ['input', 'output', 'magnitude', 'phase']
```

> **💡 Nota:** Se não houver nenhum nó selecionado, `selected_node` vale `None`. Selecionar um nó também atualiza a tabela de parâmetros lateral automaticamente.

#### Alternativa: selecionar um nó por código

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. Manipular nós e conexões sem rato

### Criar um nó novo

Deves conhecer o nome exato da classe do nó (igual ao do catálogo). Os argumentos são: `(tipo, x, y)`.

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

O nó aparece no canvas nas coordenadas (300, 200). Se não conheceres o nome exato, lista as categorias (ver secção 7).

#### Alternativa: criar vários nós de uma vez

```python
>>> for i, tipo in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(tipo, 100 + i*200, 300)
```

### Conectar nós manualmente

Sintaxe: `graph.connect_nodes(origem, destino, 'porto_saída', 'porto_entrada')`. Os portos dependem de cada nó; nunca assumas os nomes.

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 Nota:** Sempre revisa `graph.nodes[N].PORTS` antes de conectar. Um nó FFT tem `'input'` e `'magnitude'`; um gerador tem `'output'`.

#### Alternativa: conectar ao porto por defeito

Se não souberes o nome exato do porto de entrada, alguns nós aceitam `None` para usar o primeiro disponível:

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### Eliminar um nó

```python
>>> graph.remove_node(graph.nodes[2])
```

Elimina o nó e todas as suas conexões associadas. Os índices de `graph.nodes` reordenam-se, por isso não guardes referências velhas.

#### Alternativa: eliminar todos os nós de uma categoria

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### Ver os portos de um nó

```python
>>> graph.nodes[1].PORTS
```

**Exemplo de saída:**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

A chave é o nome do porto (string). O valor é um tuplo com a posição visual. Apenas as chaves te interessam para conectar.

#### Alternativa: ver portos como lista simples

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. Executar o fluxo e ver resultados

### Executar todo o grafo

```python
>>> graph.execute_flow()
```

Este método pertence ao diagrama (`graph`), não à janela principal. Percorre todos os nós em ordem topológica, executa cada um e guarda os resultados em cache. Não devolve nada; os dados ficam armazenados internamente.

#### Alternativa: forçar cálculo de uma ramo específico

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

Isto recalcula toda a árvore a montante do nó indicado e devolve o resultado diretamente, sem modificar o cache global.

### Ver os dados em cache de um nó

Se precisares de aceder aos dados processados de um nó específico, podes fazê-lo de duas formas:

#### Forma direta (por objeto)
```python
>>> graph.node_values[graph.nodes[1]]
```

**Exemplo de saída:**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ Aviso:** Esta forma pode falhar com `KeyError` se a ordem dos nós na cena mudou (por exemplo, ao eliminar ou adicionar nós) ou se a instância do objeto não coincide exatamente com a chave guardada no dicionário.

#### Forma alternativa (por posição)
```python
>>> list(graph.node_values.values())[1]
```

Esta forma é **mais estável** porque não depende da identidade exata do objeto. A ordem dos valores segue a sequência em que os nós foram executados durante o último `graph.execute_flow()`. O índice `[1]` corresponde ao segundo nó nessa sequência.

> **💡 Nota:** Se quiseres ver o índice de cada nó na ordem de execução, podes usar:
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ Cuidado:** `graph.node_values` nem sempre devolve um array direto. Para nós processadores (FFT, filtros, etc.) devolve um **dicionário** onde cada chave é um porto de saída. Para nós fonte, devolve um tuplo `(x, y)`.

#### Alternativa: ver dados de todos os nós numa linha
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### Aceder ao eixo Y de um nó fonte

Os nós geradores (SignalSourceNode, FileInputNode, etc.) devolvem um tuplo `(tempo, sinal)` ao executarem. Para obter apenas o eixo Y:

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**Exemplo de saída:**
```
Scalar NumPy (float64): 1.0
```

#### Alternativa: obter o eixo X (tempo)

```python
>>> x = result[0]
>>> x[:5]
```

### Atribuições em Python: um detalhe vital

Em Python, as atribuições (`=`) são **statements**, não expressões. A consola não imprime nada depois de `x, y = ...` porque não há valor de retorno.

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

Para verificar que funcionou, avalia a variável na linha seguinte:

```python
>>> x
>>> y.shape
```

Ou usa `;` para encadear uma expressão na mesma linha:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

Ou usa `print()` explícito:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. Desenhar na gráfica principal

### Limpar a gráfica

```python
>>> app.plot_widget.clear_plot()
```

#### Alternativa: limpar e redesenhar imediatamente

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### Desenhar um sinal arbitrário desde a consola

Podes criar arrays com NumPy e enviá-los diretamente ao widget de plotagem, sem passar por nenhum nó.

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### Alternativa: desenhar uma soma de senos

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### Desenhar o resultado de um nó fonte

Como o nó fonte devolve `(x, y)`, podes desempacotar diretamente:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### Desenhar o resultado de um nó FFT (portos múltiplos)

Os nós com múltiplas saídas (FFT, análise tempo-frequência, etc.) não devolvem um tuplo simples. Devolvem um `dict` onde cada chave é um porto de saída.

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**Exemplo de saída:**
```
dict_keys(['output', 'magnitude', 'phase'])
```

Observa que `'output'` pode ser `None` se o nó não tiver um porto genérico. As saídas úteis são `'magnitude'` e `'phase'`, que por sua vez são tuplos `(frequências, valores)`:

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### Alternativa: desenhar a fase em vez da magnitude

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### Alternativa: sobrepor dois sinais

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # outro nó
>>> app.plot_widget.plot_waveform(x2, y2)  # sobrepõe-se
```

> **💡 Nota:** Se tentares fazer `x, y = result` com um dict, Python lançará `ValueError: too many values to unpack`. Sempre inspeciona com `type(result)` e `result.keys()` antes de desempacotar.

---

## 5. Modificar a aplicação sobre a marcha

### Atualizar a tabela de parâmetros lateral

Se modificares um parâmetro por código e quiseres que a tabela lateral reflita a mudança:

```python
>>> app.workspace_table.populate()
```

Este método não recebe argumentos. Refresca a tabela com os valores atuais do nó selecionado.

#### Alternativa: forçar seleção de outro nó e refrescar

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### Adicionar um nó desde a barra de ferramentas

```python
>>> app.add_node('SignalSourceNode')
```

É equivalente a premir o botão "+" na barra de ferramentas. O nó coloca-se numa posição pré-determinada do canvas.

### Mudar o título da janela

`setWindowTitle` é o método nativo de Qt. Funciona, mas tem em conta que a aplicação pode ter um timer ou evento que chame a `update_title()` e te o sobrescreva automaticamente.

```python
>>> app.setWindowTitle('O meu laboratório de sinais')
>>> app.windowTitle()
```

Para restaurar o título "oficial" que a app calcula desde o seu estado interno (nome de projeto, ficheiro, etc.):

```python
>>> app.update_title()
```

#### Alternativa: título com nome de projeto

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. Navegação e produtividade na consola

A consola não é um simples `print()`. Tem histórico, autocompletado e blocos multilinha.

| Tecla / Comando           | Ação                                                         |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | Navegar pelo histórico de comandos executados                |
| `Tab`                     | Autocompletar variáveis, atributos e métodos do namespace     |
| `Ctrl + L`                | Limpar toda a consola (apaga texto, não o estado de Python)  |
| `if`, `for`, `def`, `class` | O prompt muda de `>>>` para `...` para blocos multilinha     |
| `Ctrl+C` (na seleção)   | Copiar texto da consola                                     |
| `Ctrl+A`                  | Selecionar todo o conteúdo                                  |

> **💡 Nota:** O autocompletado usa `rlcompleter` e reconhece todo o namespace injetado (`app`, `graph`, `selected_node`) mais qualquer variável que definires na sessão.

---

## 7. Receitas avançadas

### Mudar um parâmetro interno de um nó

Os parâmetros dos nós não são atributos planos. Estão aninhados dentro do dicionário `params`, que por sua vez tem subsecções como `'preset'`, `'formula'` ou `'advanced'`. Nunca faças `nó.amplitude = 3.0`; isso cria um atributo novo no objeto mas não modifica o parâmetro real.

#### Caso A: modificar um preset (seno, quadrada, etc.)

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Caso B: usar uma fórmula custom

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

A expressão usa `t` como variável de tempo. Os valores em `vars` são os símbolos que podes referenciar na fórmula. Se omitires `vars`, o nó usará valores por defeito e a fórmula pode não refletir a mudança.

#### Caso C: mudar parâmetros avançados (sample rate, duração)

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Caso D: mudar parâmetro de um nó não gerador (ex. FFT)

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 Nota:** `getattr(obj, '_generate_signal', lambda: None)()` é um padrão seguro: se o método existe (nós geradores), chama-o; se não, não faz nada e não lança erro. Para nós processadores, só `graph.execute_flow()` é suficiente.

### Listar todas as categorias de nós disponíveis

O import carrega o catálogo, mas não o mostra automaticamente. Lembra-te que em Python um import bem-sucedido não imprime nada; deves avaliar o objeto.

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

Para ver um resumo legível:

```python
>>> for cat, nodos in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(nodos)} nós")
```

#### Alternativa: listar nomes de nós por categoria

```python
>>> {cat: [n.__name__ for n in nodos] for cat, nodos in NODE_CATEGORIES.items()}
```

### Ver a ajuda de qualquer método

```python
>>> help(graph.connect_nodes)
```

Aparece a docstring diretamente na consola. É útil para descobrir que argumentos espera um método sem abrir o código fonte.

#### Alternativa: ver atributos filtrados

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. O que fazer se algo correr mal?

- **Erro a vermelho na consola:** o traceback mostra-se completo. A aplicação não fecha; podes corrigir o comando e voltar a tentar.
- **A interface congela:** provavelmente escreveste um ciclo infinito. A consola executa numa thread separada, mas se o ciclo afetar a GUI thread, reinicia a aplicação.
- **`None` inesperado:** se um nó devolve `None` em vez de dados, verifica que está conectado a montante (`graph.connections`) e que o fluxo foi executado (`graph.execute_flow()`).
- **`ValueError: too many values to unpack`:** estás a tentar desempacotar um dict como se fosse um tuplo. Usa `result.keys()` primeiro.
- **`ValueError: not enough values to unpack`:** esperas 2 valores mas o nó devolve 1 (dict) ou 3 (espectrograma). Inspeciona com `type(result)` antes de desempacotar.
- **`AttributeError`:** o objeto não tem esse atributo. Usa `dir(obj)` ou `[a for a in dir(obj) if 'palavra' in a.lower()]` para descobrir o nome correto.
- **Não passa nada ao executar:** verifica que há pelo menos um nó fonte conectado à cadeia e que `graph.execute_flow()` foi chamado. Os nós processadores não geram dados sozinhos.
- **O plot não muda:** assegura-te de chamar `graph.execute_flow()` depois de modificar parâmetros. Só mudar `params` não recalcula automaticamente.

---

© 2026 FloWorks — Laboratório de Sinais
