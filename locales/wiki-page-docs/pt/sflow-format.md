---
title: Formato de Ficheiro .sflow
description: Especificação técnica, estrutura interna e guia de uso do padrão de intercâmbio do FloWorks
---

# 📄 Formato de Ficheiro `.sflow`

O formato `.sflow` é o padrão nativo de intercâmbio e persistência do **FloWorks**. Permite empacotar um fluxo de trabalho completo num único ficheiro que inclui a topologia do grafo, os parâmetros dos nós, os dados processados e notas adesivas, facilitando partilhar, arquivar ou reproduzir experiências de forma determinista.

---

## 📦 O que é um ficheiro `.sflow`?

Um ficheiro `.sflow` é, em essência, um **ficheiro ZIP renomeado**. Ao alterar a sua extensão para `.zip`, pode inspecionar o seu conteúdo com qualquer gestor de ficheiros ou ferramenta de linha de comandos.

A sua estrutura interna mínima consiste em:

| Componente | Descrição |
|------------|-------------|
| `diagram.json` | Manifesto principal: define nós, ligações, vista, notas adesivas e metadados de serialização. |
| `data/` | Pasta com os dados de cada nó em formato `.npy` (arrays binários de NumPy). |
| `metadata.json` *(opcional)* | Informação complementar: autor, versão do FloWorks, descrição e etiquetas. |

=== "🌳 Estrutura Visual"
    ```text
    mi-flujo.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (opcional, para ScriptNode persist)
    ```

---

## 🧩 `diagram.json` – O Coração do Fluxo

Este ficheiro JSON descreve a topologia completa, a posição dos elementos no canvas e o estado da vista no momento de guardar.

### Exemplo Mínimo
```json
{
  "nodes": [
    {
      "id": "n1",
      "type": "OscilloscopeNode",
      "pos": [150, 200],
      "params": { "channel": "primary", "simulation": false }
    },
    {
      "id": "n2",
      "type": "GraphExporterNode",
      "pos": [450, 200],
      "params": { "theme": "dark", "export_format": "png" }
    }
  ],
  "connections": [
    {
      "from": "n1",
      "to": "n2",
      "from_port": "out",
      "to_port": "data_in"
    }
  ],
  "viewport": { "x": 0, "y": 0, "scale": 1.0 },
  "stickers": [
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "Revisar umbral", "user_modified": true }
  ]
}
```

### Campos Principais
| Campo | Tipo | Descrição |
|-------|------|-------------|
| `nodes` | `Array` | Lista de objetos `{id, type, pos, params}`. `type` deve coincidir com `node_registry.py`. |
| `connections` | `Array` | Lista de ligações `{from, to, from_port, to_port}`. As portas são cadeias, não índices. |
| `viewport` | `Object` | `(x, y, scale)` para restaurar exatamente a posição e zoom do canvas. |
| `stickers` | `Array` | Notas adesivas serializadas com coordenadas normalizadas a 96 dpi. |

!!! tip "Serialização de Nós"
    Os parâmetros específicos de cada nó são geridos através de `SerializableMixin`. Apenas os atributos declarados em `SERIALISABLE = [...]` são guardados. Os nós não registados no sistema são omitidos automaticamente durante o carregamento.

---

## 💾 `data/` – Dados Processados e Arrays NumPy

Quando um fluxo é executado, os nós podem armazenar os seus resultados em ficheiros `.npy` dentro desta pasta.

- O nome do ficheiro costuma coincidir com o `id` do nó ou com referências internas.
- Os arrays são armazenados em formato binário de NumPy, **preservando estritamente a dimensionalidade original** (1D, 2D, 3D, etc.). O motor nunca aplica `flatten()`.
- Em `diagram.json`, os dados são referenciados com o prefixo `__npy__:`:
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- Se um nó não produz dados ou está configurado para não os persistir, o ficheiro correspondente pode ser omitido.

??? note "Compatibilidade Externa"
    Os ficheiros `.npy` são universais no ecossistema Python. Pode lê-los fora do FloWorks com:
    ```python
    import numpy as np
    datos = np.load("data/node_1.npy")
    print(datos.shape)
    ```

---

## 🏷️ `metadata.json` (Opcional)

Contém informação descritiva que não afeta a execução, ideal para rastreabilidade e gestão de projetos:

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "Análisis de vibraciones en motor trifásico",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["ingeniería", "vibraciones", "FFT", "multicanal"]
}
```

---

## 🔄 Processo de Guardar e Carregar

O FloWorks implementa um mecanismo robusto para garantir a integridade dos dados:

1. **Guardar:**
   - Percorre-se o grafo e serializam-se os nós via `SerializableMixin`.
   - Os arrays são extraídos para `data/` e referenciados em JSON com `__npy__:`.
   - Tudo é empacotado num ZIP com extensão `.sflow`.
2. **Carregamento Seguro:**
   - Cria-se uma **cópia de segurança temporária em memória** do diagrama atual.
   - Extrai-se e analisa-se o novo `.sflow`.
   - Se ocorrer qualquer erro (JSON inválido, nós em falta, corrupção de `.npy`), **restaura-se automaticamente a cópia de segurança** sem perda de trabalho.
3. **ScriptNode Especial:**
   - Guarda `script`, `params`, `dynamic_inputs`, `dynamic_outputs`, `persist` e `python_path`.
   - Ao carregar, recompila o código, reconstrói as portas dinâmicas e restaura o estado `persist` automaticamente.
4. **StickyNotes e DPI:**
   - As coordenadas e tamanhos são normalizados a **96 dpi** ao guardar.
   - Ao carregar, escalam-se ao DPI do monitor atual, garantindo consistência visual entre diferentes resoluções.

---

## 🛠️ Uso Externo e Automatização

O formato `.sflow` está concebido para ser transparente e programático. Pode lê-lo ou gerá-lo a partir de scripts externos:

=== "🐍 Python (Leitura)"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("mi-flujo.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        datos_n1 = np.load(z.open("data/node_1.npy"))
        print(f"Nodos: {len(graph['nodes'])}")
        print(f"Datos: {datos_n1.shape}")
    ```

=== "📤 Python (Criação Básica)"
    ```python
    import zipfile
    import json
    import numpy as np

    graph = {
        "nodes": [{"id": "gen", "type": "GeneratorNode", "pos": [100, 100], "params": {}}],
        "connections": [],
        "viewport": {"x": 0, "y": 0, "scale": 1.0}
    }

    with zipfile.ZipFile("nuevo.sflow", "w", zipfile.ZIP_DEFLATED) as z:
        z.writestr("diagram.json", json.dumps(graph, indent=2))
        z.writestr("data/gen.npy", np.array([1.0, 2.0, 3.0]))
    ```

---

## 🔮 Compatibilidade e Extensibilidade Futura

O formato `.sflow` segue os princípios de **design extensível e retrocompatível**:

- ✅ **Novas secções:** Versões futuras podem adicionar pastas como `thumbnails/`, `logs/` ou `plugins/` sem quebrar carregadores antigos.
- ✅ **Campos opcionais:** O parser ignora chaves desconhecidas em `diagram.json`, permitindo adicionar metadados experimentais.
- ✅ **Versionamento:** O campo `floworks_version` em `metadata.json` permite à aplicação aplicar migrações automáticas se o formato evoluir.

!!! warning "Regra de Ouro"
    Nunca modifique manualmente `diagram.json` enquanto a aplicação está aberta. O sistema depende da consistência entre a topologia, os arrays e o estado da vista. Utilize sempre os fluxos de guardar/carregar nativos.

---

## 📚 Recursos Relacionados
- [🗺️ Mapa de Código e Arquitetura](architecture-ii.md) → Como `file_io.py` e `SerializableMixin` gerem o formato.
- [📦 Guia de Build Portable](guia-ejecutable-portable.md) → Empacotamento e caminhos seguros para recursos.
- [🧩 Referência de Nós](node-reference.md) → Contratos de serialização por tipo de nó.
