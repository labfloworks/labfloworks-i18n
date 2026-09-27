---
title: Referência Técnica de Nós
description: Catálogo atualizado, contratos de extensão e capacidades avançadas do sistema de nós do FloWorks
---

# 🧩 Referência Técnica de Nós

O FloWorks não depende de um catálogo estático. Utiliza um **sistema de registo dinâmico** baseado em contratos claros. Isto permite alargar a plataforma sem tocar no motor topológico. Segue-se o catálogo implementado, as capacidades técnicas reais e o protocolo para o alargar de forma segura.

---

## 📂 Categorias do Núcleo

=== "📦 Vista por Camadas"
    <div class="grid cards" markdown>

    - **📥 Fontes/Entrada**
      Geram ou capturam sinais iniciais. Suportam simulação integrada, hardware real (VISA/SCPI) e modo multicanal.
    - **⚙️ Processamento**
      Transformam, combinam ou analisam dados. Preservam dimensionalidade e interpolam automaticamente quando necessário.
    - **🔀 Controlo/Fluxo**
      Bifurcam, iteram ou condicionam a execução. Incluem suporte nativo para sinais de ativação.
    - **🐍 Scripting/Avançado**
      Executam código Python dinâmico com portos paramétricos (`# @param`), portos dinâmicos (`# @input`/`# @output`) e persistência de estado (`persist`).
    - **🔌 Hardware/Instrumentação**
      Interfaces para osciloscópios, medidores LCR e geradores.
    - **📤 Saída/Exportação**
      Visualizam, exportam ou arquivam resultados. Suportam temas visuais, perfis de utilizador e formato profissional (PNG/PDF/SVG).

    </div>

---

## 📋 Catálogo Técnico Implementado

| Nó | Tipo | Responsabilidade Principal | Características Clave |
|------|------|---------------------------|------------------------|
| `SumNode` | Processamento | Operador aritmético (+, -, *, /) para duas entradas. | Interpola automaticamente sinais de distinta resolução (FFTs). Preserva dimensionalidade. |
| `RhombusNode` | Controlo | Condicional (bifurcação Sim/Não). | Dois portos de saída. Avalia condição por limiar ou lógica booleana. |
| `TriggerNode` | Controlo | Iterador/acumulador com ativação externa. | Recebe `(x, y, "trigger")`. Acumula até N iterações e emite resultado empilhado/médio. |
| `ScriptNode` | Avançado | Ambiente de scripting Python integrado. | QScintilla, autocompletado, `# @param`, portos dinâmicos, `persist`, modelos, consola de erros, intérprete externo com timeout. |
| `OscilloscopeNode` | Hardware | Captura desde osciloscópios (SDS) ou medidores LCR. | Modo simulação, diálogo de firewall integrado, **suporte multicanal** (`out_primary`, `out_secondary`), menu "Mostrar canal". |
| `GeneratorNode` | Fonte | Envia sinais a geradores (SDG) ou simula saídas. | Configuração de modulação/varredura, diálogo de simulação integrado. |
| `GraphExporterNode` | Saída | Exportador de gráficos profissionais. | Configuração por duplo clique, eixos personalizados, temas, perfis guardados, etc. |

---

## 🔍 ScriptNode: Capacidades Essenciais

> **🐍 Ambiente de Scripting Integrado**
>
> - **Editor de código integrado:** Destaque de sintaxe básico, numeração de linhas e dobramento de código.
> - **Painel de parâmetros dinâmicos:** Diretivas `# @param NOME : tipo = valor` injetam controlos editáveis (spinbox, campo de texto, etc.) no painel lateral.
> - **Portos dinâmicos:** `# @input nome` e `# @output nome` criam portos em tempo real. O script recebe um dicionário `inputs` e devolve `outputs`.
> - **Persistência de estado:** Dicionário global `persist` que mantém valores entre execuções.
> - **Modelos e Import/Export:** Menu pendente com scripts base. O utilizador pode guardar os seus scripts em `nodes/script_node/templates/` ou importar/exportar ficheiros `.py` externos.
> - **Consola de erros integrada:** Mostra falhas de sintaxe/execução com a linha exata assinalada no editor.
> - **Ajuda e i18n:** Tooltips contextuais, botão `?` com guia rápida, e todos os textos usam `tr()` para tradução.
> - **Intérprete externo com timeout:** Caminho configurável (`# @python_path` ou botão "Examinar…"). Execução isolada com limite de tempo e fallback ao intérprete interno.
> - **Serialização completa:** Guarda script, parâmetros, portos dinâmicos e estado `persist`. Ao carregar um `.sflow`, reconstrói automaticamente portos e parâmetros.

---

## 📚 Recursos Relacionados

- [📖 Mapa de Código e Arquitetura](architecture-ii.md) → Responsabilidades por módulo e fluxos de trabalho.
- [🌐 Guia de Internacionalização (i18n)](i18n.md) → Como adicionar idiomas e gerir chaves `tr()`.
- [🛠️ Adicionar um Novo Nó (Tutorial)](adding-a-new-node.md) → Passo a passo com exemplos práticos.
- [📦 Guia de Build e Distribuição](build.md) → Empacotamento PyInstaller, hooks e assinaturas digitais.

---

💡 **Falta um nó neste catálogo?**  
O FloWorks está desenhado para ser extensível. Se precisas de um nó que não existe, cria-o seguindo o contrato de `BaseNode` e regista-o. A comunidade e o futuro marketplace ampliarão continuamente o ecossistema sem quebrar compatibilidade.
