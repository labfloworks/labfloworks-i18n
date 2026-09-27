---
title: Primeiros passos com o FloWorks
description: Guia rápido para configurar o ambiente, executar o seu primeiro fluxo e aceder à versão portátil.
---

# 🚀 Primeiros passos com o FloWorks

Este guia levará você desde o zero até ter o seu primeiro fluxo de processamento de sinais em execução. O FloWorks é uma aplicação de diagramas de fluxo para sinais, construída com Python e PySide6, que suporta hardware real (VISA/SCPI), simulação integrada, scripting avançado e mudança de idioma em tempo real.

---

## 🌊 O seu primeiro fluxo de exemplo

Vamos criar um fluxo simples: gerar um sinal senoidal e visualizá-lo em tempo real.

1. **Adicionar nós**
   Na barra de ferramentas superior, selecione `Fonte` → selecione `Gerador de sinais Avançado`. Depois, em `Processamento` → selecione, por exemplo, `Espectral`.
2. **Ligar**
   Faça `Ctrl+Clique` na porta de saída (`direita`) do gerador. Depois, faça `Clique` na porta de entrada (`esquerda`) do osciloscópio. Ou simplesmente clique na porta de saída e arraste (mantendo o clique pressionado) até à porta de entrada do nó seguinte.
3. **Configurar (opcional)**
   Clique num nó, no painel lateral esquerdo aparecerá um editor com os parâmetros do nó que tem selecionado para ajustar as condições de operação. Na parte inferior há um gráfico de visualização que representa visualmente os dados gerados ou adquiridos pelos nós.
4. **Executar**
   Pressione `F5` ou o botão ▶ na toolbar. O motor topológico calculará a ordem de execução, processará os dados e verá a onda no painel gráfico. O conector animar-se-á indicando o fluxo ativo!

---

## 🧠 Entendendo as portas: categorias por cor

No FloWorks, cada porta pertence a uma **categoria funcional** identificada por uma cor. As ligações válidas fazem-se **sempre entre portas da mesma cor**: uma saída de uma categoria liga-se unicamente com uma entrada da mesma categoria. Além disso, a linha conectora adota automaticamente a cor das portas que une, facilitando a leitura visual.

| Tipo | Cor | Propósito | Exemplo típico |
|------|-------|-----------|----------------|
| `control` | Branco | Fluxo de controlo / ativação. | Sinal de arranque para um nó de aquisição. |
| `exec` | Cinzento | Execução de operações ou passos. | Disparo de uma função ou callback. |
| `data` | Verde | Dados genéricos / sinais numéricos. | Saída de um gerador ou sensor. |
| `int` | Azul | Números inteiros. | Índice, tamanho de buffer, ID. |
| `float` | Ciano | Números de vírgula flutuante. | Amplitude, frequência, limiar. |
| `string` | Púrpura | Cadeias de texto. | Nome de ficheiro, etiqueta. |
| `bool` | Rosa | Valores booleanos (`True`/`False`). | Indicador de estado, habilitação. |
| `array` | Azul escuro | Arranjos / vetores. | Sinal multicanal, lista de amostras. |
| `trigger` | Laranja | Disparos / eventos discretos. | Pulso de sincronização, flanco. |

**Regra de ouro:**

- Só se ligam portas da **mesma cor exata** (saída ↔ entrada da mesma categoria).
- O sistema evita ligações inválidas e realça visualmente as portas compatíveis ao arrastar.
- A linha conectora toma a cor das portas ligadas; assim cada percurso identifica-se de um relance.

**Filosofia FloWorks:**  
As portas de dados **preservam a dimensionalidade** dos arrays. Nunca se aplica um achatamento automático: se entra uma matriz, sai uma matriz, mantendo a integridade dos seus sinais multidimensionais.

![FloWorks](assets/tipos_de_puertos.PNG)

---

## 🖱️ Navegação pelo Canvas

Domine o espaço de trabalho com estes gestos:

| Ação | Como fazer |
|--------|--------------|
| **Zoom** | Roda do rato ou `Ctrl + roda` |
| **Paneamento (deslocar)** | Mantenha `Espaço` e arraste, ou use o botão do meio do rato |
| **Selecionar um nó** | Clique esquerdo sobre o nó |
| **Seleção múltipla** | Arraste um retângulo com clique esquerdo, ou `Ctrl + clique` em vários nós |
| **Mover seleção** | Arraste qualquer um dos nós selecionados |
| **Abrir configuração** | Duplo clique sobre um nó |

**Conselho:** O painel esquerdo atualiza-se automaticamente com a configuração do nó selecionado, sem necessidade de abrir janelas adicionais.

---

## ⚡ Atalhos de teclado e movimentos avançados

Estes atalhos convertem um utilizador normal num **power user**:

| Atalho | Ação |
|-------|--------|
| `F5` | Executar fluxo |
| `Ctrl + S` | Guardar projeto (`.sflow`) |
| `Ctrl + Clique` | Ligar nós (clique na porta de saída → clique na porta de entrada) |
| `Ctrl + C` / `Ctrl + V` | Copiar / colar nós selecionados |
| `Ctrl + Z` / `Ctrl + Y` | Desfazer / refazer |
| `Ctrl + Shift + L` | Auto-organizar nós no canvas |
| `Supr` | Eliminar nós selecionados |
| `Ctrl + A` | Selecionar todos os nós |

**Movimentos avançados:**

- **Duplicar um fluxo:** selecione um grupo de nós, `Ctrl + C`, `Ctrl + V` e arraste a cópia para outra zona.
- **Limpar a grelha:** use `Ctrl + Shift + L` para ordenar todo o canvas com um único comando.
- **Ligação rápida:** `Ctrl + Clique` numa porta de saída e depois clique normal na porta de entrada; o FloWorks desenha a ligação automaticamente.

---

## 🎨 Personalização do ambiente

O FloWorks adapta-se a si, não o contrário.

### Mudança de tema em tempo real
Desde a barra superior, menu **Ver → Tema**, escolha entre claro, escuro ou outros. A interface muda **instantaneamente**, sem reiniciar nem perder o fluxo de trabalho.

### Tamanho da letra
Em **Ver → Tamanho da letra** selecione um valor predefinido ou personalizado. Toda a interface ajusta-se no momento.

### Idioma
Em **Ver → Idioma** selecione o idioma desejado. O FloWorks suporta **mudança em tempo real**: os menus, botões e mensagens traduzem-se sem reiniciar a aplicação.

---

## ❗ Solução de problemas comuns

| Problema | Possível causa | Solução |
|----------|---------------|----------|
| O fluxo não se executa | Há nós sem configurar ou ligações partidas | Verifique que todos os nós têm parâmetros válidos e que as ligações são entre portas compatíveis |
| O gráfico não se atualiza | O fluxo está em pausa ou não há dados a fluir | Certifique-se de que pressionou `F5` ou ▶, e de que os nós de origem estão a gerar dados |
| Não consigo ligar dois nós | As portas são de tipo diferente | Verifique que ambas as portas são de **dados** ou ambas de **controlo** |
| O programa vai lento com fluxos grandes | Demasiados nós ou gráficos em tempo real | Feche painéis de análise não usados ou reduza a frequência de amostragem dos nós fonte |
| O tema não muda | Alguns widgets podem não estar registados | Reinicie a aplicação e tente novamente (em versões futuras estará resolvido) |

---

## 🧪 Exemplos práticos rápidos

Além do fluxo senoidal inicial, experimente estes mini-projetos para dominar o FloWorks:

| Exemplo | Nós envolvidos | Resultado esperado |
|---------|-------------------|--------------------|
| **Filtro passa-baixo** | Gerador → Filtro → Visualizador de Gráficos | Verá o sinal filtrado |
| **Aquisição simulada** | Gerador → Analisador de THD | valor da distorção harmónica do sinal |
| **Controlo manual** | Gerador → Inspetor de dados | tabela com os valores do sinal enviado pelo gerador |
| **Comparação de sinais** | Dois geradores → Somador → Visualizador de Gráficos | o resultado da operação (soma, subtração, multiplicação ou divisão) de duas ondas num único gráfico |

Cada um destes fluxos pode ser montado em menos de um minuto, demonstrando a agilidade do FloWorks face ao código tradicional.

---

## 📚 O que segue?

| Recurso | Descrição |
|---------|-------------|
| [🗺️ Guia de Anatomia da Interface](interface-anatomy.md) | Entendimento da arquitetura e filosofia da interface gráfica |
| [🗺️ Mapa de Código e Arquitetura](philosophy.md) | Estrutura completa, managers, contratos e DPI-Awareness. |
| [🧩 Referência Técnica de Nós](node-reference.md) | Catálogo, `ScriptNode`, multicanal e como estender o sistema. |
| [🌐 Guia de Internacionalização](translation-guide.md) | Adicionar idiomas, validar JSON e gerir chaves `tr()`. |
| [📦 Guia de Build Portable](guia-ejecutable-portable.md) | PyInstaller, hooks, `--onefile`, solução de erros e assinatura digital. |

---

!!! warning "Notas de compatibilidade e uso"
    1. **Versão de Python:** Pode usar 3.9+, e sistemas de 64 bits.
    2. **Firewall do Windows:** Se usa hardware real (osciloscópio VISA/SCPI), permita `FloWorks.exe` no firewall. A app mostra um diálogo personalizado se a ligação for bloqueada (o diálogo do SO não aparece em modo `--windowed`).
    3. **Atalhos chave:** `F5` (executar), `Ctrl+S` (guardar `.sflow`), `Ctrl+Clique` (ligar), `Espaço+clique` (paneamento livre), `Ctrl+Shift+L` (auto-layout).
    4. **Preservação de dados:** O motor **nunca** aplica `flatten()` aos arrays. Trabalhe com cópias locais se precisar de vetorizar.
