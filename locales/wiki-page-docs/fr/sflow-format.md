---
title: Format de Fichier .sflow
description: Spécification technique, structure interne et guide d'utilisation du standard d'échange de FloWorks
---

# 📄 Format de Fichier `.sflow`

Le format `.sflow` est le standard natif d'échange et de persistance de **FloWorks**. Il permet d'empaqueter un flux de travail complet dans un fichier unique qui inclut la topologie du graphe, les paramètres des nœuds, les données traitées et les notes adhésives, facilitant le partage, l'archivage ou la reproduction déterministe des expériences.

---

## 📦 Qu'est-ce qu'un fichier `.sflow` ?

Un fichier `.sflow` est, en essence, un **fichier ZIP renommé**. En changeant son extension en `.zip`, vous pouvez inspecter son contenu avec n'importe quel gestionnaire de fichiers ou outil en ligne de commandes.

Sa structure interne minimale se compose de :

| Composant | Description |
|-----------|-------------|
| `diagram.json` | Manifeste principal : définit les nœuds, connexions, vue, notes adhésives et métadonnées de sérialisation. |
| `data/` | Dossier avec les données de chaque nœud au format `.npy` (tableaux binaires NumPy). |
| `metadata.json` *(optionnel)* | Informations complémentaires : auteur, version de FloWorks, description et étiquettes. |

=== "🌳 Structure Visuelle"
    ```text
    mon-flux.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (optionnel, pour ScriptNode persist)
    ```

---

## 🧩 `diagram.json` – Le Cœur du Flux

Ce fichier JSON décrit la topologie complète, la position des éléments sur le canevas et l'état de la vue au moment de l'enregistrement.

### Exemple Minimal
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
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "Vérifier seuil", "user_modified": true }
  ]
}
```

### Champs Principaux
| Champ | Type | Description |
|-------|------|-------------|
| `nodes` | `Array` | Liste d'objets `{id, type, pos, params}`. `type` doit correspondre à `node_registry.py`. |
| `connections` | `Array` | Liste de connexions `{from, to, from_port, to_port}`. Les ports sont des chaînes, pas des indices. |
| `viewport` | `Object` | `(x, y, scale)` pour restaurer exactement la position et le zoom du canevas. |
| `stickers` | `Array` | Notes adhésives sérialisées avec coordonnées normalisées à 96 dpi. |

!!! tip "Sérialisation des Nœuds"
    Les paramètres spécifiques de chaque nœud sont gérés via `SerializableMixin`. Seuls les attributs déclarés dans `SERIALISABLE = [...]` sont sauvegardés. Les nœuds non enregistrés dans le système sont automatiquement omis lors du chargement.

---

## 💾 `data/` – Données Traitées et Tableaux NumPy

Lorsqu'un flux est exécuté, les nœuds peuvent stocker leurs résultats dans des fichiers `.npy` à l'intérieur de ce dossier.

- Le nom du fichier correspond généralement à l'`id` du nœud ou à des références internes.
- Les tableaux sont stockés au format binaire NumPy, **préservant strictement la dimensionalité originale** (1D, 2D, 3D, etc.). Le moteur n'applique jamais `flatten()`.
- Dans `diagram.json`, les données sont référencées avec le préfixe `__npy__:` :
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- Si un nœud ne produit pas de données ou est configuré pour ne pas les persister, le fichier correspondant peut être omis.

??? note "Compatibilité Externe"
    Les fichiers `.npy` sont universels dans l'écosystème Python. Vous pouvez les lire en dehors de FloWorks avec :
    ```python
    import numpy as np
    donnees = np.load("data/node_1.npy")
    print(donnees.shape)
    ```

---

## 🏷️ `metadata.json` (Optionnel)

Contient des informations descriptives qui n'affectent pas l'exécution, idéales pour la traçabilité et la gestion de projets :

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "Analyse de vibrations dans moteur triphasé",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["ingénierie", "vibrations", "FFT", "multicanal"]
}
```

---

## 🔄 Processus de Sauvegarde et Chargement

FloWorks implémente un mécanisme robuste pour garantir l'intégrité des données :

1. **Sauvegarde :**
   - Le graphe est parcouru et les nœuds sont sérialisés via `SerializableMixin`.
   - Les tableaux sont extraits vers `data/` et référencés en JSON avec `__npy__:`.
   - Tout est empaqueté dans un ZIP avec extension `.sflow`.
2. **Chargement Sécurisé :**
   - Une **sauvegarde temporaire en mémoire** du diagramme actuel est créée.
   - Le nouveau `.sflow` est extrait et parsé.
   - Si une erreur survient (JSON invalide, nœuds manquants, `.npy` corrompu), **la sauvegarde est automatiquement restaurée** sans perte de travail.
3. **ScriptNode Spécial :**
   - Sauvegarde `script`, `params`, `dynamic_inputs`, `dynamic_outputs`, `persist` et `python_path`.
   - Au chargement, il recompile le code, reconstruit les ports dynamiques et restaure l'état `persist` automatiquement.
4. **StickyNotes & DPI :**
   - Les coordonnées et tailles sont normalisées à **96 dpi** lors de la sauvegarde.
   - Au chargement, elles sont mises à l'échelle au DPI du moniteur actuel, garantissant la cohérence visuelle entre différentes résolutions.

---

## 🛠️ Utilisation Externe et Automatisation

Le format `.sflow` est conçu pour être transparent et programmatique. Vous pouvez le lire ou le générer depuis des scripts externes :

=== "🐍 Python (Lecture)"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("mon-flux.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        donnees_n1 = np.load(z.open("data/node_1.npy"))
        print(f"Nœuds : {len(graph['nodes'])}")
        print(f"Données : {donnees_n1.shape}")
    ```

=== "📤 Python (Création Basique)"
    ```python
    import zipfile
    import json
    import numpy as np

    graph = {
        "nodes": [{"id": "gen", "type": "GeneratorNode", "pos": [100, 100], "params": {}}],
        "connections": [],
        "viewport": {"x": 0, "y": 0, "scale": 1.0}
    }

    with zipfile.ZipFile("nouveau.sflow", "w", zipfile.ZIP_DEFLATED) as z:
        z.writestr("diagram.json", json.dumps(graph, indent=2))
        z.writestr("data/gen.npy", np.array([1.0, 2.0, 3.0]))
    ```

---

## 🔮 Compatibilité et Extensibilité Future

Le format `.sflow` suit les principes de **conception extensible et rétrocompatible** :

- ✅ **Nouvelles sections :** Les versions futures peuvent ajouter des dossiers comme `thumbnails/`, `logs/` ou `plugins/` sans casser les chargeurs anciens.
- ✅ **Champs optionnels :** Le parser ignore les clés inconnues dans `diagram.json`, permettant d'ajouter des métadonnées expérimentales.
- ✅ **Versionnage :** Le champ `floworks_version` dans `metadata.json` permet à l'application d'appliquer des migrations automatiques si le format évolue.

!!! warning "Règle d'Or"
    Ne modifiez jamais manuellement `diagram.json` pendant que l'application est ouverte. Le système dépend de la cohérence entre la topologie, les tableaux et l'état de la vue. Utilisez toujours les flux de sauvegarde/chargement natifs.

---

## 📚 Ressources Liées
- [🗺️ Carte du Code et Architecture](architecture-ii.md) → Comment `file_io.py` et `SerializableMixin` gèrent le format.
- [📦 Guide de Build Portable](guia-ejecutable-portable.md) → Empaquetage et chemins sûrs pour les ressources.
- [🧩 Référence des Nœuds](node-reference.md) → Contrats de sérialisation par type de nœud.
