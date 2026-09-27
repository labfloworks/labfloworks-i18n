# Filosofia

## Visão geral

O FloWorks é uma aplicação de ambiente de trabalho que permite criar cadeias de processamento de sinais através de diagramas de fluxo visuais.  
Arraste, ligue e configure nós; o resultado é calculado e apresentado em tempo real.  
Trabalhe com sinais simulados ou ligue instrumentos reais (osciloscópios, geradores, multímetros LCR) sem necessidade de escrever código, embora disponha de um potente ambiente de scripting se desejar ampliar a funcionalidade.

---

## Características principais

- **Diagramas interativos** – Construa o seu fluxo de trabalho unindo nós com linhas que representam o fluxo de dados.
- **Processamento em tempo real** – Cada modificação reflete-se imediatamente nos gráficos e visualizações.
- **Simulação e hardware real** – Gere sinais de teste ou capture dados diretamente desde instrumentos de laboratório.
- **Nó de script avançado** – Incorpore o seu próprio código Python com ajuda de autocompletar, parâmetros dinâmicos editáveis e memória persistente entre execuções.
- **Visualização profissional** – Sinais, espetros, espetrogramas e gráficos de alta qualidade prontos para exportar.
- **Multilingue** – A interface deteta o idioma do sistema e permite mudar entre espanhol, inglês e outros idiomas em qualquer momento.
- **Temas visuais** – Modo escuro, claro e de alto contraste para se adaptar às suas preferências ou necessidades de acessibilidade.
- **Gestão completa de projetos** – Guarde o seu trabalho em ficheiros `.sflow` e recupere-o exatamente como o deixou, com desfazer e refazer ilimitados.

---

## Como trabalhar com o FloWorks

### Nós
Um nó é uma peça do processamento. Organizam-se em três categorias:

- **Fontes** – Inserem sinais no início do fluxo. Por exemplo, um osciloscópio (real ou simulado), um gerador de funções ou uma operação matemática.
- **Processamento** – Transformam os dados. Somas, subtrações, condicionais, filtros… inclusive um nó especial para escrever os seus próprios scripts em Python.
- **Sumidouros** – Mostram ou exportam os resultados. O visualizador gráfico e o exportador de gráficos profissionais são os mais utilizados.

### Ligações
As uniões entre nós são desenhadas como curvas suaves ou linhas ortogonais. Uma animação de fluxo indica em todo o momento a direção dos dados. O sistema organiza automaticamente os cabos para que não se sobreponham.

### Visualização
Cada vez que um nó produz um sinal, este pode ser visto no painel de gráficos integrado. Pode explorar diferentes representações (forma de onda, espetro, espetrograma) e ajustar a escala com o rato.

---

## Nós destacados
São os nós mínimos indispensáveis, necessários para que a filosofia do programa faça sentido.

### Nó gerador de sinais
Fonte de sinais que pode gerar simulações personalizadas de formas de onda ao gosto do utilizador. Permite desde um menu de contexto selecionar ou digitar a forma de onda pretendida.

### Nó de script
Um ambiente de programação completo dentro do diagrama:

- **Editor com realce de sintaxe**, autocompletar e consola de erros.
- **Parâmetros dinâmicos** – Defina variáveis editáveis desde o painel do nó sem modificar o código.
- **Portas configuráveis** – Adicione entradas e saídas adicionais diretamente desde o editor.
- **Estado persistente** – Guarde valores entre execuções; tudo é armazenado junto com o projeto.

### Exportador de gráficos
Nó sumidouro que gera imagens de alta qualidade para relatórios ou publicações. Permite configurar tamanho, resolução, formato, entre outros.

---

## Personalização

- **Idioma** – A aplicação deteta automaticamente o idioma do sistema e guarda a sua preferência. Pode mudá-lo desde o menu sem reiniciar.
- **Aspeto** – Escolha entre tema escuro, claro ou de alto contraste consoante a luz ambiente ou as suas necessidades visuais.

---

## Projetos e ficheiros

Guarde o seu diagrama completo num ficheiro `.sflow`.  
Ao abri-lo recuperará todos os nós, ligações, scripts, parâmetros e configurações de visualização.  
As ações de desfazer e refazer permitem-lhe experimentar sem medo de perder o trabalho anterior.

---

O FloWorks está concebido para que se concentre na análise de sinais e não nos detalhes técnicos da implementação. Arraste, ligue e descubra.
