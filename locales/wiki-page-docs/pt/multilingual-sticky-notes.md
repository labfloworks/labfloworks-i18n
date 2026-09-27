# Sticky Notes (Notas adesivas) – Guia do utilizador

## O que são as notas adesivas?

As notas adesivas (ou *sticky notes*) são pequenos blocos de texto que pode colocar livremente sobre o diagrama. Servem para:

- Adicionar lembretes, títulos ou explicações diretamente no canvas.
- Criar tutoriais passo a passo que guiem quem utiliza o seu projeto.
- Documentar partes do fluxo de trabalho sem necessidade de sair do FloWorks.
- Deixar comentários para si próprio ou para outros colaboradores.

As notas são redimensionáveis (arrastando os cantos), movem-se para qualquer lugar do diagrama e são guardadas juntamente com o projeto. Ao abrir um ficheiro `.sflow`, todas as notas aparecem exatamente onde as deixou.

---

## A novidade: notas multilíngue

As notas adesivas podem mostrar automaticamente o texto no idioma que escolheu para a aplicação.  
Em vez de escrever a mensagem final num único idioma, pode inserir **marcadores especiais** que se traduzirão automaticamente ao alterar o idioma do FloWorks.

Deste modo, a mesma nota pode ser lida em espanhol, inglês ou qualquer outro idioma disponível sem necessidade de editar o texto cada vez.

---

## Como escrever uma nota multilíngue

Dentro de uma nota (crie-a com duplo clique ou com o botão 📝 da barra de ferramentas), pode usar dois tipos de marcadores:

### 1. Com a palavra `tr(…)`
Escreva `tr("chave")` e substitua `chave` por um nome descritivo da frase.

Exemplo:

```
tr("tutorial.paso1.titulo")
tr("tutorial.paso1.mensaje")
```

### 2. Com chaves duplas `{{…}}`
Escreva `{{chave}}` da mesma forma.

Exemplo:

```
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Ambos os formatos funcionam igual; escolha o que lhe for mais confortável (pode até combiná-los na mesma nota).

> **Importante**: O texto que vê quando edita a nota contém os marcadores originais (por exemplo `{{tutorial.paso1.titulo}}`).  
> Ao terminar de editar e voltar à vista normal do diagrama, os marcadores são substituídos pela frase traduzida para o idioma atual da aplicação.

---

## Comportamento ao alterar o idioma

- Se alterar o idioma a partir do menu do FloWorks (por exemplo, de espanhol para inglês), **todas as notas adesivas que contenham marcadores atualizam-se automaticamente**.
- Não é necessário fechar e voltar a abrir o projeto, nem tocar em cada nota manualmente.
- As notas que só contêm texto normal (sem marcadores) não são afetadas; mostram o mesmo em qualquer idioma.

---

## Vantagens de usar marcadores

- **Tutoriais multilíngue instantâneos** – A mesma nota serve para guiar utilizadores de diferentes idiomas.
- **Coesão** – Se modificar a tradução num único local (o ficheiro de idiomas que a sua equipa de desenvolvimento mantém), todas as notas que usam essa chave serão atualizadas.
- **Manutenção simples** – Pode escrever o conteúdo uma única vez e reutilizá-lo em múltiplas notas.
- **Flexibilidade** – Combine texto fixo com marcadores. Por exemplo:

```
🎯 PASSO 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

---

## Exemplo prático: um tutorial passo a passo

Suponha que quer adicionar uma nota que explique o primeiro passo de um tutorial.  
Em modo de edição escreve:

```
🎯 PASSO 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Quando terminar de editar e estiver a usar a aplicação em espanhol, verá:

```
🎯 PASO 1
¡Bienvenido a FloWorks!
Arrastre un nodo fuente de señal para comenzar.
```

Se alterar o idioma para inglês, a mesma nota mostrará:

```
🎯 STEP 1
Welcome to FloWorks!
Drag a signal source node to begin.
```

E assim para qualquer outro idioma que tenha configurado.

---

## Resumo

- As notas adesivas enriquecem os seus diagramas com informação textual.
- Agora podem ser **multilíngues** usando os marcadores `tr("chave")` ou `{{chave}}`.
- Ao editar vê as chaves; ao visualizar, o texto traduzido.
- Altere o idioma da aplicação e todas as notas adaptam-se instantaneamente.
- Perfeito para criar documentação visual, tutoriais ou avisos que devem funcionar em vários idiomas.

Aproveite esta funcionalidade para tornar os seus projetos mais acessíveis e fáceis de partilhar!
