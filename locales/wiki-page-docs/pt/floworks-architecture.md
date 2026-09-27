---
title: Arquitetura do FloWorks
description: Visão geral dos componentes e funcionamento interno para o utilizador final
---

# Arquitetura do FloWorks – Visão para o utilizador

O FloWorks é uma aplicação de ambiente de trabalho que permite construir cadeias de processamento de sinais através de diagramas de fluxo. Liga blocos (nós) num canvas interativo e vê os resultados em tempo real. Para tornar isto possível, a aplicação está organizada em vários módulos que trabalham em conjunto. A seguir, sem detalhes técnicos, explica-se o que faz cada parte e como se relacionam entre si.

---

## Estrutura geral

A aplicação é composta pelas seguintes áreas funcionais:

| Área | O que faz? |
|------|------------|
| **Início e janela principal** | Arranca o programa, mostra a janela, os menus e coordena todas as ações do utilizador. |
| **Motor de execução** | Calcula a ordem em que os nós devem ser executados, deteta dependências e ciclos, e transmite os dados de um nó para outro. |
| **Cena e diagrama** | Gere o canvas onde coloca os nós, as ligações entre eles, os post-its e as ações de desfazer/refazer. |
| **Nós e processamento** | Contém todos os tipos de blocos que pode usar: fontes de sinal, operações matemáticas, scripts personalizados, exportação de gráficos, etc. |
| **Conetores visuais** | Desenha as linhas que unem os nós (curvas suaves ou ortogonais), anima-as para mostrar o fluxo de dados e evita que se sobreponham. |
| **Interface do utilizador** | Inclui a vista do diagrama (zoom, deslocamento), a barra de ferramentas, a tabela de parâmetros, os painéis de análise (estatísticas, cursores) e os diálogos de configuração. |
| **Suporte para hardware real** | Permite a comunicação com instrumentos de laboratório (osciloscópios, geradores, multímetros LCR) para capturar ou gerar sinais reais. |
| **Exportação de gráficos** | Gera imagens de alta qualidade (PNG, PDF, SVG) com total personalização visual. |
| **Temas e aspeto** | Altera o aspeto de toda a aplicação (escuro, claro, alto contraste) e permite ajustar o tamanho da letra. |
| **Idiomas** | Traduz toda a interface para vários idiomas e permite mudar de idioma instantaneamente. |
| **Gestão de projetos** | Guarda e abre ficheiros `.sflow` com todo o diagrama, incluindo configurações, scripts e resultados. |
| **Testes e diagnóstico** | Ferramentas internas para verificar que tudo funciona corretamente (não visíveis para o utilizador final). |

---

## Como funciona por dentro

### Arranque e janela principal
Ao abrir o FloWorks, configura-se o ambiente gráfico, deteta-se a densidade de pixéis do ecrã (para que tudo se veja nítido em monitores 4K ou normais) e mostra-se a janela principal. Esta janela centraliza todos os elementos: a área de desenho, os menus, a barra de ferramentas e os painéis laterais.

### Motor de fluxos
Quando o utilizador prime "Executar" (ou pressiona F5), um motor interno percorre todos os nós na ordem correta, respeitando as ligações. Sabe quais os nós que dependem de outros e evita os ciclos infinitos. Suporta que um nó receba várias entradas nomeadas e que produza múltiplas saídas. Os dados viajam entre nós sem perder a sua estrutura original.

### Cena do diagrama
O canvas onde constrói os seus diagramas é uma cena inteligente:

- Permite adicionar nós, movê-los, ligá-los e selecioná-los.
- Suporta desfazer e refazer ilimitados para qualquer ação.
- Inclui post-its redimensionáveis que pode colocar livremente e que são guardados com o projeto.
- Dispõe de um organizador automático que recoloca os nós ordenadamente (com Ctrl+Shift+L).
- Ao guardar, todo o diagrama é empacotado num ficheiro `.sflow` que contém as descrições dos nós, as ligações, os post-its e os dados numéricos associados.

### Conetores
As linhas que unem os nós são desenhadas como curvas suaves ou trajetórias ortogonais. Uma suave animação de pontos ou traços indica a direção do fluxo. Um gestor de faixas evita que várias ligações entre os mesmos nós se amontonem; separa-as automaticamente para que tudo seja legível.

### Tipos de nós
Os nós são as peças fundamentais. Agrupam-se em três categorias:

- **Fontes** – Geram sinais. Podem simular ondas (sinusoidal, quadrada, etc.) ou ler dados reais desde um osciloscópio ou multímetro ligado. Suportam múltiplos canais simultâneos (por exemplo, impedância e fase desde um LCR).
- **Processamento** – Transformam os dados. Incluem operações aritméticas (soma, subtração, multiplicação, divisão), decisões condicionais (bifurcação Sim/Não) e um potente nó de script que permite escrever o seu próprio código Python com ajudas visuais.
- **Sumidouros** – Mostram ou exportam os resultados. O mais comum é o visualizador gráfico (osciloscópio virtual), mas também existe um exportador de gráficos de qualidade profissional.

Cada nó tem portas de entrada (esquerda/cima) e de saída (direita/baixo). Ao ligar uma porta de saída a uma de entrada, o sinal flui entre eles.

#### Nó de script avançado
O nó de script merece menção especial. Está pensado para utilizadores avançados que queiram adicionar o seu próprio processamento sem sair do FloWorks. Oferece:

- Um editor com realce de sintaxe, autocompletar e numeração de linhas.
- A possibilidade de definir parâmetros editáveis desde o painel do nó sem tocar no código (por exemplo, um valor numérico que depois é usado no script).
- Portas de entrada e saída dinâmicas: adicionando comentários especiais no script, pode criar novos conetores.
- Memória persistente: uma variável especial (`persist`) que conserva o seu valor entre execuções, útil para acumuladores ou máquinas de estado.
- Modelos de scripts já preparados e a opção de guardar os seus próprios.
- Um sistema de ajuda integrado e uma consola que mostra erros de execução.

### Interface do utilizador
Além do canvas, a interface inclui:

- Uma **barra de ferramentas** com todos os nós organizados por categorias, menus de idioma, tema e tamanho de letra, e acesso ao visualizador de registos.
- Uma **tabela de parâmetros** que mostra informação dos nós selecionados e realça possíveis incompatibilidades (como tentar operar com sinais de comprimento diferente).
- **Painéis de análise** acopláveis: estatísticas (máximo, mínimo, valor eficaz), cursores A/B para medir diferenças, e um ponto de mira com marcador de pico.
- Um **diálogo de boas-vindas** que se adapta à resolução do ecrã e oferece opções iniciais.

### Ligação com instrumentos reais
Se dispuser de hardware compatível (osciloscópios Siglent SDS, multímetros LCR, geradores SDG), o FloWorks pode comunicar-se com eles através do protocolo padrão VISA/SCPI. A configuração realiza-se a partir de painéis específicos dentro da aplicação. Quando captura um sinal multicanal (por exemplo, magnitude e fase de um LCR), o nó fonte empacota todos os canais e pode escolher qual visualizar com um simples menu de contexto.

### Exportação de gráficos profissionais
O nó exportador de gráficos permite gerar imagens prontas para relatórios ou publicações. Ao fazer duplo clique sobre ele, abre-se um diálogo com múltiplas opções: pode personalizar cores, tipos de linha, etiquetas, escalas, escolher entre formatos PNG, PDF ou SVG, e guardar as suas preferências como perfis reutilizáveis.

### Personalização visual
O FloWorks inclui vários temas (escuro, claro, alto contraste) que alteram o aspeto de toda a aplicação instantaneamente, sem reiniciar. Além disso, pode ajustar o tamanho de letra global a partir do menu (Informação → Tamanho da letra) e todos os elementos são redimensionados em conformidade, incluindo os textos dentro dos nós, os post-its e os gráficos.

### Sistema de idiomas
A aplicação deteta automaticamente o idioma do sistema ao iniciar pela primeira vez e guarda a sua preferência. Pode mudar de idioma em qualquer momento a partir do menu; todos os textos, menus e ajudas são atualizados em tempo real.

### Projetos e ficheiros `.sflow`
Todo o seu trabalho é guardado num único ficheiro com extensão `.sflow`. Este ficheiro contém o diagrama completo: nós, ligações, post-its, configurações, scripts e os dados numéricos gerados. Pode partilhá-lo com outros utilizadores; ao abri-lo noutro equipamento, os post-its e os nós são automaticamente reescalados para se adaptarem à densidade de pixéis desse ecrã.

---

## Fluxos de trabalho típicos

1. **Criar um diagrama simples**  
   Selecione um nó fonte (p. ex., Gerador) e um nó Visualizador desde a barra de ferramentas.  
   Ligue a saída do gerador à entrada do visualizador (Ctrl+clique na porta de saída, depois clique na de entrada).  
   Prima F5 para executar. Verá o sinal no gráfico.

2. **Usar um script personalizado**  
   Adicione um nó Script.  
   Escreva o seu código Python no editor; pode definir parâmetros editáveis e portas extra.  
   Ligue as suas entradas e saídas como qualquer outro nó.  
   Execute o fluxo; o script é processado com os seus dados.

3. **Capturar dados de um osciloscópio real**  
   Ligue o instrumento e configure a comunicação desde o painel do nó Osciloscópio.  
   O nó adquire o sinal e entrega-o pelas suas portas de saída (uma por canal).  
   Ligue essas portas a outros nós de processamento ou ao visualizador.

4. **Exportar um gráfico para um relatório**  
   Ligue o sinal desejado a um nó Exportador de gráficos.  
   Selecione no nó (clique direito) para configurar o aspeto visual do gráfico.  
   Também se podem carregar/guardar perfis para agilizar a obtenção de gráficos prontos para relatórios, obtendo o ficheiro de imagem na extensão escolhida.

---

## Para que serve tudo isto

Esta arquitetura está pensada para que se possa concentrar na análise de sinais sem se preocupar com a organização interna do programa. Cada componente tem uma função clara e trabalha em conjunto para oferecer uma experiência fluida, desde a simulação até à instrumentação real, passando pela personalização visual e pela exportação de resultados.

Se alguma vez precisar de ampliar as capacidades do FloWorks (por exemplo, adicionando novos tipos de nós ou ligando um instrumento diferente), saiba que existe uma estrutura modular que o permite, embora esse seja terreno para programadores. Como utilizador final, desfrute da flexibilidade que este desenho proporciona.
