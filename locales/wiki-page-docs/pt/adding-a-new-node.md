---
title: Guia para Adicionar um Novo Nó ao FloWorks
description: Tutorial passo a passo para criar, registar e integrar nós personalizados no motor de fluxos do FloWorks.
---

# 📘 Guia para Programadores: Como Adicionar um Novo Nó ao FloWorks

Este guia descreve o processo completo para criar um novo tipo de nó no FloWorks, garantindo que se integre corretamente com o motor de fluxos, a interface do utilizador, os temas visuais e o sistema de internacionalização.

---

## 📋 Índice
- [📘 Guia para Programadores: Como Adicionar um Novo Nó ao FloWorks](#-guia-para-programadores-como-adicionar-um-novo-nó-ao-floworks)
  - [📋 Índice](#-índice)
  - [1. Introdução à Arquitetura](#1-introdução-à-arquitetura)
  - [2. Utilizar o Modelo `template_node.py`](#2-utilizar-o-modelo-template_nodepy)
  - [3. Passo a Passo: Criação de um Nó Personalizado](#3-passo-a-passo-criação-de-um-nó-personalizado)
    - [3.1. Copiar e Renomear o Modelo](#31-copiar-e-renomear-o-modelo)
    - [3.2. Definir Portas e Etiquetas](#32-definir-portas-e-etiquetas)
    - [3.3. Implementar a Lógica de Processamento](#33-implementar-a-lógica-de-processamento)
    - [3.4. Personalizar a Aparência (Opcional)](#34-personalizar-a-aparência-opcional)
    - [3.5. Adicionar Parâmetros Configuráveis (Opcional)](#35-adicionar-parâmetros-configuráveis-opcional)
    - [3.6. Tornar o nó serializável (guardar / carregar configurações)](#36-tornar-o-nó-serializável-guardar--carregar-configurações)
  - [4. Integração no Sistema](#4-integração-no-sistema)
  - [5. Internacionalização (i18n)](#5-internacionalização-i18n)
  - [6. Temas Visuais](#6-temas-visuais)
  - [7. Lista de Verificação e Resolução de Problemas](#7-lista-de-verificação-e-resolução-de-problemas)
    - [✅ Lista de Verificação](#-lista-de-verificação)
    - [🐛 Problemas Comuns](#-problemas-comuns)
  - [8. Conclusão](#8-conclusão)

---

## 1. Introdução à Arquitetura

O FloWorks é construído sobre o PySide6 e utiliza um modelo de nós conectáveis que representam um fluxo de processamento de sinais.

---

## 2. Utilizar o Modelo `template_node.py`

Para facilitar a criação de novos nós, é fornecido o ficheiro `nodes/template_node.py`. Este modelo inclui:
- Suporte completo para internacionalização (ligação a `languageChanged`, método `update_language`).
- Suporte completo para temas (método `update_theme`).
- Ajuda integrada com formato HTML de três secções.
- Gestão de múltiplas portas de entrada/saída configuráveis.
- Múltiplas saídas com `get_output_for_port`.
- Visualização no gráfico através de `get_display_signal`.
- Menu contextual traduzível.

Recomenda-se partir sempre deste modelo ao desenvolver um novo nó.

---

## 3. Passo a Passo: Criação de um Nó Personalizado

### 3.1. Copiar e Renomear o Modelo
1. Copie `nodes/template_node.py` com o nome do seu novo nó, por exemplo `nodes/mi_nodo.py`.
2. Renomeie a classe de `TemplateNode` para algo descritivo, por exemplo `MiNodoNode`.
3. Ajuste os imports se necessário.

### 3.2. Definir Portas e Etiquetas
!!! warning "Importante: Correspondência de Nomes"
    Os nomes das portas em `PORTS`, `PORT_LABELS` e as chaves do dicionário devolvido por `execute_program` devem ser **exatamente iguais** (incluindo maiúsculas/minúsculas). O modelo agora inclui um mapeamento de alias (`'data_in'` → primeira porta esquerda) para maior robustez.

Edite o dicionário `PORTS` na parte superior do ficheiro. Cada entrada tem o formato:
```python
"nome_porta": ("lado", fração)
```
- **Lados possíveis:** `"left"`, `"right"`, `"top"`, `"bottom"`.
- **Fração:** valor entre `0.0` e `1.0` que indica a posição ao longo do lado.

**Exemplo para um nó com uma entrada e duas saídas:**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
O dicionário `PORT_LABELS` contém o texto que aparecerá junto a cada porta. Recomenda-se usar chaves de tradução em vez de texto fixo (ver secção Internacionalização).

### 3.3. Implementar a Lógica de Processamento
O método chave é `execute_program(self, input_data)`. Este método é invocado pelo motor de fluxos quando o nó recebe dados.

**`input_data` pode ser:**
- `None` se não houver entrada.
- Um tuplo `(x, y)` para sinais temporais.
- Um array 1D.
- Um dicionário `{nome_porta: dados}` em nós com múltiplas entradas.

**Valor de retorno:**
- Para nós com uma única saída, devolva diretamente os dados (por exemplo, tuplo `(x, y)`).
- Para nós com múltiplas saídas, devolva um dicionário onde as chaves coincidem com os nomes das portas de saída definidas em `PORTS`.

```python
def execute_program(self, input_data):
    # Processar input_data e gerar resultados
    resultado_magnitud = (freq, mag)
    resultado_fase = (freq, phase)
    return {
        "magnitude": resultado_magnitud,
        "phase": resultado_fase
    }
```

!!! tip "Nota sobre nomes de porta genéricos"
    O motor de fluxos pode passar ocasionalmente um dicionário com chaves como `'data_in'` em vez do nome real da porta (especialmente se o utilizador não clicou exatamente no círculo). O modelo já inclui código para lidar com este caso:
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    Isto evita que o nó falhe por um erro de ligação impreciso.

O modelo já inclui um exemplo comentado. Além disso, implemente `get_output_for_port(self, port_name)` para que o motor possa encaminhar cada saída:
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. Personalizar a Aparência (Opcional)
O método `paint()` desenha o fundo, título, estado e qualquer texto adicional. Pode modificar:
- As cores (atualizam-se automaticamente com `update_theme`).
- O texto de estado (usando o atributo `self._status`).
- Informação de resumo (por exemplo, pico de magnitude).

O modelo mostra um exemplo básico.

### 3.5. Adicionar Parâmetros Configuráveis (Opcional)
Se o seu nó requer parâmetros ajustáveis pelo utilizador (por exemplo, tamanho da janela, frequência de corte), pode:
1. Adicionar atributos em `__init__` (por exemplo, `self.window_size = 512`).
2. Criar um diálogo de configuração (herde de `QDialog`).
3. Ligar o diálogo em `open_config_dialog()` (método já presente no modelo).
4. Atualizar os parâmetros a partir do diálogo e chamar `self.update()`.

### 3.6. Tornar o nó serializável (guardar / carregar configurações)
Para que o nó possa guardar e recuperar os seus parâmetros ao copiar/colar, desfazer/refazer, ou ao usar os comandos Guardar/Abrir do menu Ficheiro, deve herdar do mixin de serialização e declarar os seus atributos.

1. Importe o mixin no seu ficheiro:
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. Altere a herança da classe para o incluir antes de `QGraphicsObject`:
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
    ```
3. Defina a lista `SERIALISABLE` a nível de classe, com os nomes dos atributos que quer persistir. Apenas tipos simples (`int`, `float`, `str`, `bool`), listas, dicionários, ou arrays NumPy são admitidos (estes últimos são armazenados automaticamente como ficheiros `.npy` dentro do `.sflow`).
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frecuencia', 'amplitud', 'configuracion']
    ```
4. Certifique-se de que esses atributos são inicializados em `__init__`:
    ```python
    self.frecuencia = 1000.0
    self.amplitud = 1.0
    self.configuracion = {'tipo': 'seno', 'fase': 0}
    ```

Com isto, não precisa de escrever métodos `serialize`/`deserialize`; o mixin trata automaticamente de guardar e recuperar os valores.

Se o seu nó requer lógica adicional ao carregar (por exemplo, reconectar um instrumento hardware), pode sobrescrever `deserialize` chamando primeiro ao método pai:
```python
def deserialize(self, data):
    super().deserialize(data)   # restaura os atributos de SERIALISABLE
    self._iniciar_dispositivo()
```

---

## 4. Integração no Sistema

Uma vez criado o ficheiro do nó, apenas precisa de colá-lo na pasta `nodes` para que apareça na interface e funcione com o resto do sistema.

---

## 5. Internacionalização (i18n)

Todos os textos visíveis devem ser traduzíveis através de `tr("chave", default="...")`. O modelo já o implementa. Deve adicionar as chaves correspondentes nos ficheiros JSON dentro de `locales/`.

**Estrutura recomendada:**
```json
{
   "nodes": {
     "mi_nodo": {
       "title": "Mi Nodo",
       "tooltip": "Descripción emergente",
       "ports": {
         "input": "Entrada",
         "output1": "Salida 1",
         "output2": "Salida 2"
      },
       "status": {
         "no_data": "Sin datos",
         "ready": "Listo"
      },
       "menu": {
         "show_output": "Mostrar salida",
         "configure": "Configurar..."
      },
       "help_title": "Ayuda - Mi Nodo",
       "help_html": "<h3>🎛️ Filter Node</h3>\n<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_mi_nodo": "Mi Nodo"
  }
}
```

A ajuda HTML segue o formato de três secções comum a todos os nós (descrição específica + "Como pensar no sistema" + "Atalhos e truques"). O modelo já inclui a estrutura em `get_help_text()`.

---

## 6. Temas Visuais

O método `update_theme(self, theme)` recebe um dicionário com as cores definidas pelo tema atual. O modelo atualiza automaticamente:
- Fundo do nó (`node_normal_bg`)
- Borda (`node_selected_border`)
- Cor do título e texto (`node_normal_text`)
- Cores das portas (`port_circle`, `port_outline`, `port_inline`, `port_text`)

Certifique-se de que em `MainWindow` (ou `ThemeUpdater`) se chama `node.update_theme()` para cada nó quando o tema muda.

---

## 7. Lista de Verificação e Resolução de Problemas

### ✅ Lista de Verificação
- [ ] O nó é criado corretamente a partir da barra de ferramentas.
- [ ] As portas são apresentadas nas posições esperadas e são detetáveis para ligações (`Ctrl+clique`).
- [ ] Ao receber dados de entrada, `execute_program` é chamado e o sinal é processado.
- [ ] As saídas propagam-se corretamente para os nós ligados.
- [ ] O menu contextual permite alterar o canal de visualização (se houver múltiplas saídas).
- [ ] Ao clicar no nó, o sinal selecionado é desenhado no widget de gráfico.
- [ ] O duplo clique abre a ajuda com o formato adequado.
- [ ] O idioma muda corretamente (textos de título, portas, menus).
- [ ] O tema muda corretamente (cores do nó e das portas).
- [ ] Copiar/colar funciona sem erros.

!!! tip "Ligação precisa de portas"
    Ao ligar nós, certifique-se de clicar exatamente sobre o círculo da porta de destino. Se clicar no corpo do nó, o sistema usará um nome genérico (`'data_in'`). O modelo agora tolera estes nomes, mas é uma boa prática ligar diretamente ao círculo para garantir o encaminhamento correto de múltiplas saídas.

### 🐛 Problemas Comuns

| Sintoma | Causa Possível | Solução |
|---------|----------------|---------|
| A seta de ligação não se ancora à porta. | O círculo da porta não tem `setData(0, port_name)` ou `get_port_scene_pos` não está implementado. | Verificar que em `_create_ports` se faz `circle.setData(0, port_name)` e que `get_port_scene_pos` usa esse nome. |
| As saídas não chegam aos nós ligados. | `execute_program` não devolve um dicionário (para múltiplas saídas) ou `get_output_for_port` não está implementado. | Assegurar que `execute_program` devolve `{nome_porta: dados}` e que `get_output_for_port` devolve o valor correspondente. |
| Ao clicar no nó nada é desenhado no gráfico. | `get_display_signal` não devolve um tuplo `(x, y)` válido ou `display_channel` não coincide com uma saída existente. | Revisar que `get_display_signal` usa o canal selecionado e que os dados são arrays NumPy. |
| Os textos não se atualizam ao mudar idioma. | Não se ligou o sinal `languageChanged` ou `update_language` não atualiza os elementos. | Verificar a ligação em `__init__`: `language_manager.languageChanged.connect(self.update_language)`. |
| O tema não se aplica. | Não se chama `update_theme` ao criar o nó ou ao mudar tema. | Em `MainWindow`, depois de criar o nó, invocar `node.update_theme(self.theme_manager.current_theme())`. |
| A seta aponta ao centro do nó. | Fez-se clique no corpo em vez do círculo, ou o nome não coincide com `PORTS`. | Faça clique diretamente sobre o círculo. Verifique que `get_port_scene_pos` tem o mapeamento de alias. |
| `NameError: name 'self' is not defined` ao importar. | Declararam-se atributos de instância fora de `__init__`. | Todos os atributos como `self.mi_parametro` devem ser definidos dentro de `__init__`. |
| Parâmetros perdem-se ao copiar/abrir `.sflow`. | O nó não herda de `SerializableMixin` ou não definiu `SERIALISABLE`. | Implementar o passo 3.6 deste guia. |

---

## 8. Conclusão

Seguindo este guia e utilizando o modelo `template_node.py`, poderá adicionar novos nós ao FloWorks de forma eficiente e coerente com o resto do sistema. Lembre-se sempre de manter a compatibilidade com i18n e temas para uma experiência de utilizador profissional.

Anime-se a contribuir com os seus próprios nós!
