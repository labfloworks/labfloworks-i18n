---
title: FloWorks
description: Laboratório visual universal para processamento de sinais, instrumentação científica e automação.
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## Laboratório visual universal para sinais, instrumentação e IA

Processamento científico • DSP • VISA/SCPI • Automação • Machine Learning

![Captura do FloWorks](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[Primeiros passos com o FloWorks](getting-started.md){ .md-button }
[Anatomia da interface](interface-anatomy.md){ .md-button .md-button--primary }
[Filosofia](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## O que é o FloWorks?
O FloWorks é um **laboratório visual de código aberto** (Python + PySide6) onde constrói sistemas conectando blocos (nós) em vez de escrever linhas de código.

Imagine um canvas digital onde une geradores de sinais, filtros matemáticos, controladores de hardware (VISA/SCPI) e modelos de Inteligência Artificial mediante cabos virtuais. Tudo se baseia no **fluxo de dados**: conecta a saída de um bloco com a entrada de outro para processar informação, automatizar equipamentos ou analisar resultados em tempo real.

Está orientado a estudantes, investigadores, engenheiros e qualquer pessoa que queira experimentar, aprender ou prototipar sistemas complexos de forma intuitiva, sem a barreira da programação tradicional.

### Missão
Centralizar o fluxo de trabalho experimental numa única ferramenta visual, aberta e acessível. Queremos que os utilizadores se concentrem em *experimentar e descobrir*, não em lutar contra a complexidade do software ou os custos das licenças.

### Visão
Um mundo onde a única barreira entre uma ideia experimental e a sua execução seja a curiosidade do experimentador. O FloWorks aspira a ser a plataforma de referência para a ciência e a tcnica, construída por e para a comunidade global, eliminando os muros das ferramentas privadas.

### Princípios
* **Liberdade Total (Licença MIT):** O conhecimento e as ferramentas devem ser livres e acessíveis para todos.
* **Extensibilidade Infinita:** Se falta um bloco, qualquer pessoa pode criá-lo e integrá-lo no ecossistema usando Python.
* **Transparência Visual:** Cada passo do processo pode ser inspecionado, depurado e entendido graficamente.
* **Conexão com o Mundo Real:** Não é apenas simulação; permite controlar instrumentação científica real diretamente desde o canvas.

Ao contrário de ferramentas fechadas ou altamente especializadas, o FloWorks está concebido como um ecossistema modular extensível onde cada componente é um nó reutilizável e conectável.

---

## Capacidades principais

<div class="grid cards" markdown>

-   **:material-puzzle-outline: Ecossistema de Nós Extensível**

    Catálogo técnico organizado em camadas: Fontes, Processamento, Controlo, Hardware e Scripting.

    Registo dinâmico, serialização declarativa e contratos claros para desenvolvimento rápido.

    [:material-arrow-right: Referência de Nós](node-reference.md)

-   **:material-connection: Integração VISA/SCPI**

    Conexão direta com osciloscópios, LCR meters e geradores.

    Suporte multicanal, simulação integrada via `PyVISA-py` e gestão de firewall em modo portable.

    [:material-arrow-right: Instrumentação](instrumentation.md)

-   **:material-package-variant-closed: Formato Portable `.sflow`**

    Padrão ZIP autocontido com grafo JSON, arrays `.npy` e metadados.

    Reproducibilidade total de experiências e normalização DPI automática.

    [:material-arrow-right: Formato .sflow](sflow-format.md)

-   **:material-translate: Internacionalização Avançada**

    Mudança de idioma em tempo real sem reiniciar a app.

    Traduções JSON hierárquicas e persistência de preferências.

    [:material-arrow-right: Guia i18n](translation-guide.md)

-   **:material-tools: SDK e Desenvolvimento Rápido**

    Modelo base (`template_node.py`), mixin de serialização e guias passo a passo.

    Arquitetura preparada para plugins e expansão comunitária.

    [:material-arrow-right: Criar Nós](adding-a-new-node.md)

</div>

---

## Áreas de aplicação

| Área | Aplicações |
|------|--------------|
| 🎓 **Educação** | Física, eletrónica, matemáticas, laboratórios STEM |
| ⚙️ **Engenharia** | DSP, controlo, instrumentação, metrologia |
| 🤖 **IA** | ML, otimização, pipelines híbridos |
| 🔬 **Investigação** | Automação e aquisição de dados |
| 🔌 **Hardware** | VISA/SCPI, simulação e sistemas híbridos |

---

!!! tip "Novo no FloWorks?"

    Comece pela secção **Primeiros passos com o FloWorks**, depois **Anatomia da interface** para entender a arquitetura da interface gráfica e finalmente explore **Arquitetura Geral** para entender o fluxo de dados e a estrutura do motor topológico.

---

!!! info "Modelo Open Core"

    O FloWorks utiliza um modelo **Free/Open Core** sob licença **MIT License**.

    O núcleo permanece livre e aberto, enquanto futuras extensões empresariais, curriculares ou marketplace serão opcionais.

---

<div markdown="1" style="text-align: center;">

## FloWorks

Processamento visual • Instrumentação • Ciência • IA

<small>Documentação construída com MkDocs Material</small>

</div>
