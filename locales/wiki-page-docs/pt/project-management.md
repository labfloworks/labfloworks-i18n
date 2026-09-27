---
title: Gestão de projetos
description: Como guardar, abrir, exportar e proteger os seus fluxos no FloWorks.
---

# 📁 Gestão de projetos

O FloWorks guarda os seus fluxos em ficheiros com a extensão **`.sflow`**. Estes ficheiros contêm toda a informação do projeto: nós, ligações, configuração e notas adesivas.

---

## Criar, abrir e guardar

| Ação | Menu | Atalho |
|--------|------|-------|
| **Novo projeto** | Ficheiro → Novo | `Ctrl + N` |
| **Abrir projeto** | Ficheiro → Abrir | `Ctrl + O` |
| **Guardar** | Ficheiro → Guardar | `Ctrl + S` |
| **Guardar como…** | Ficheiro → Guardar como… | `Ctrl + Shift + S` |

**Regra de Ouro:**  
Os fluxos são **completamente compatíveis entre todas as versões** do FloWorks (Core, Lite, Pro). Não precisa de converter nem modificar nada: apenas abrir e executar.

---

## Exportação e importação

- Para **partilhar um fluxo**, copie o ficheiro `.sflow` para outro equipamento.
- Para **trazer um fluxo externo**, use **Ficheiro → Abrir** e selecione o ficheiro.
- Se precisar de **exportar dados numéricos** (por exemplo, para CSV), utilize a ferramenta **Folha de Cálculo** do painel lateral e guarde a tabela a partir daí.

---

## Recuperação ante encerramentos inesperados

O FloWorks **não guarda automaticamente**. Por isso é importante:

- Guardar com frequência (`Ctrl + S`), especialmente antes de executar fluxos com hardware real.
- Se a aplicação se fechar de forma inesperada, as alterações não guardadas poderão ser perdidas.
- Para trabalhar com total tranquilidade, habitue-se a guardar depois de cada modificação importante.

---

## Organização recomendada

- Crie uma pasta por cada projeto ou cliente, e guarde aí todos os `.sflow` relacionados.
- Utilize **notas adesivas** dentro do canvas para documentar secções do fluxo.
- Atribua **nomes descritivos aos nós** (duplo clique → nome) para que seja mais fácil encontrar e entender o fluxo semanas depois.

---

## Boas práticas

- Antes de executar um fluxo com instrumentos reais, guarde o ficheiro.
- Se trabalha em equipa, use um sistema de controlo de versões (Git, cópias manuais) para não sobrescrever fluxos importantes.
- Faça cópias de segurança de fluxos de calibração ou diagnóstico críticos.

---

> **Conselho:** Um fluxo bem organizado e guardado é a base de um trabalho profissional no FloWorks. Não subestime o poder de um nome claro e uma pasta organizada.
