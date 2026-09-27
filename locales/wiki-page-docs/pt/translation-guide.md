---
title: Guia de Internacionalização (i18n)
description: Instruções passo a passo para adicionar e gerir traduções no FloWorks
---

#  Guia de Internacionalização (i18n)

Este documento explica como adicionar um novo idioma ao FloWorks e gerir eficazmente os ficheiros de tradução.

---

## ➕ Como Adicionar um Novo Idioma

### Passo 1: Criar o Ficheiro JSON
Navegue até à pasta `locales/`. Copie `en.json` e renomeie-o usando o código de duas letras [ISO 639-1](https://es.wikipedia.org/wiki/ISO_639-1) correspondente (por exemplo, `fr.json` para francês, `de.json` para alemão).

### Passo 2: Traduzir as Cadeias de Texto
Abra o novo ficheiro JSON num editor de texto.

!!! warning "Não Modificar as Chaves"
    **Nunca altere as chaves** (o lado esquerdo de cada par). Apenas traduza os valores (o lado direito).

**Original (`en.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "File",
    "edit": "Edit"
  }
}
```

**Exemplo Traduzido (`es.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "Archivo",
    "edit": "Editar"
  }
}
```
Certifique-se de que a chave raiz `"language_name"` contém o nome nativo do idioma (por exemplo, `"Français"`, `"Deutsch"`, `"Español"`).

### Passo 3: Validar o JSON
Verifique que o ficheiro é JSON válido (sem vírgulas finais, aspas corretas, escapes apropriados). Pode usar validadores online como [JSONLint](https://jsonlint.com) ou executar:
```bash
python -m json.tool locales/es.json
```

### Passo 4: Testar o Novo Idioma
1. Inicie o FloWorks.
2. Vá a **INFO → Idioma** e selecione o novo idioma.
3. Verifique que todos os elementos da interface se atualizam imediatamente (menus, painéis, diálogos, etiquetas de nós, etc.).

### Passo 5: Deteção Automática (Opcional)
Se a configuração regional do sistema do utilizador coincidir com o código do novo idioma, o FloWorks usará-o automaticamente no primeiro início (desde que não tenha sido guardada uma preferência anterior em `QSettings`).

---

## 🌍 Idiomas Disponíveis
- **Inglês** (`en`) – Idioma base / de fallback
- **Espanhol** (`es`)

---

## ⚙️ Notas Importantes e Boas Práticas

!!! info "Mecanismo de Fallback"
    O idioma base é o **inglês**. Se faltar uma chave de tradução num ficheiro de idioma, o FloWorks usa automaticamente a cadeia em inglês como fallback.

!!! warning "Prevenção de Transbordamento da UI"
    Mantenha as traduções concisas para evitar ruturas de layout. Se um texto traduzido for significativamente mais longo, considere abreviar ou confie no sistema de temas para lidar com o escalonamento dinâmico.

!!! tip "Preservar HTML e Placeholders"
    - **Etiquetas HTML:** Conserve todas as etiquetas HTML exatamente como estão (por exemplo, `<h3>`, `<b>`, `<pre>`, `<br>`).
    - **Placeholders:** Mantenha a sintaxe `{variable}` onde for usada (por exemplo, `"Idioma alterado para: {name} ({code})"`). Não os reordene nem elimine.

---

## 🔗 Documentação Relacionada
- [📖 Mapa de Código e Arquitetura](architecture-ii.md)
- [📦 Guia de Build e Distribuição](build.md)
- [🧩 Referência de Nós](node-reference.md)
