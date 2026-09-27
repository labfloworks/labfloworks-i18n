# 🧪 Tutoriel Interactif de la Console FloWorks

Bienvenue au laboratoire d'expérimentation de FloWorks. Cette section s'adresse à des utilisateurs plus avancés ; il s'agit d'un terminal **Python** pour contrôler tout ce qui est lié au canevas, c'est-à-dire les nœuds et leurs connexions, de manière séquentielle et ligne par ligne. C'est un terminal lié au programme qui peut le contrôler et déterminer des comportements ou des routines pour les utilisateurs les plus exigeants.

Ce guide vous montrera **pas à pas** comment contrôler et analyser vos diagrammes de flux sans toucher la souris. Chaque exemple a été validé dans la console interactive et reflète la structure réelle des données du programme.

---

## 1. Connaître le terrain

La console injecte trois objets globaux : `app` (fenêtre principale), `graph` (scène/diagramme) et `selected_node` (nœud actuellement sélectionné sur le canevas). Toutes les commandes partent de ces trois objets.

### Voir tous les nœuds

```python
>>> graph.nodes
```

**Exemple de sortie :**
```
Nœuds dans la scène :
  [0] Générateur de Signaux Avancé (type : SignalSourceNode, catégorie : Sources)
  [1] FFT (type : FFTNode, catégorie : Processing)
```

L'index entre crochets (`[0]`, `[1]`) est votre principal moyen d'accéder à un nœud. L'ordre est celui de la création sur le canevas.

#### Alternative : compter les nœuds ou filtrer par type

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### Voir toutes les connexions

```python
>>> graph.connections
```

**Exemple de sortie :**
```
Connexions dans la scène :
  [0] Générateur de Signaux Avancé (out) → FFT (input)
```

La sortie montre le nom du nœud source, le port de sortie, la flèche, le nœud de destination et le port d'entrée. Si une connexion n'apparaît pas, le flux ne pourra pas être exécuté.

#### Alternative : voir les connexions d'un seul nœud

```python
>>> selected_node.connectors
```

### Voir le nœud sélectionné

Cliquez sur un nœud du canevas puis exécutez :

```python
>>> selected_node
```

**Exemple de sortie :**
```
Nœud : FFT
  Type : FFTNode
  Catégorie : Processing
  Ports : ['input', 'output', 'magnitude', 'phase']
```

> **💡 Note :** Si aucun nœud n'est sélectionné, `selected_node` vaut `None`. Sélectionner un nœud met également à jour le tableau de paramètres latéral automatiquement.

#### Alternative : sélectionner un nœud par code

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. Manipuler les nœuds et les connexions sans souris

### Créer un nouveau nœud

Vous devez connaître le nom exact de la classe du nœud (identique au catalogue). Les arguments sont : `(type, x, y)`.

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

Le nœud apparaît sur le canevas aux coordonnées (300, 200). Si vous ne connaissez pas le nom exact, listez les catégories (voir section 7).

#### Alternative : créer plusieurs nœuds à la fois

```python
>>> for i, tipo in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(tipo, 100 + i*200, 300)
```

### Connecter des nœuds manuellement

Syntaxe : `graph.connect_nodes(origine, destination, 'port_sortie', 'port_entrée')`. Les ports dépendent de chaque nœud ; ne supposez jamais leurs noms.

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 Note :** Vérifiez toujours `graph.nodes[N].PORTS` avant de connecter. Un nœud FFT a `'input'` et `'magnitude'` ; un générateur a `'output'`.

#### Alternative : connecter au port par défaut

Si vous ne connaissez pas le nom exact du port d'entrée, certains nœuds acceptent `None` pour utiliser le premier disponible :

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### Supprimer un nœud

```python
>>> graph.remove_node(graph.nodes[2])
```

Supprime le nœud et toutes ses connexions associées. Les index de `graph.nodes` sont réordonnés, donc ne conservez pas d'anciennes références.

#### Alternative : supprimer tous les nœuds d'une catégorie

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### Voir les ports d'un nœud

```python
>>> graph.nodes[1].PORTS
```

**Exemple de sortie :**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

La clé est le nom du port (chaîne). La valeur est un tuple avec la position visuelle. Seules les clés vous intéressent pour la connexion.

#### Alternative : voir les ports comme une liste simple

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. Exécuter le flux et voir les résultats

### Exécuter tout le graphe

```python
>>> graph.execute_flow()
```

Cette méthode appartient au diagramme (`graph`), pas à la fenêtre principale. Elle parcourt tous les nœuds en ordre topologique, exécute chacun et met en cache les résultats. Elle ne renvoie rien ; les données restent stockées en interne.

#### Alternative : forcer le calcul d'une branche spécifique

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

Cela recalcule tout l'arbre en amont du nœud indiqué et renvoie le résultat directement, sans modifier le cache global.

### Voir les données en cache d'un nœud

Si vous devez accéder aux données traitées d'un nœud spécifique, vous pouvez le faire de deux manières :

#### Manière directe (par objet)
```python
>>> graph.node_values[graph.nodes[1]]
```

**Exemple de sortie :**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ Avertissement :** Cette manière peut échouer avec `KeyError` si l'ordre des nœuds dans la scène a changé (par exemple, en supprimant ou ajoutant des nœuds) ou si l'instance de l'objet ne correspond pas exactement à la clé stockée dans le dictionnaire.

#### Manière alternative (par position)
```python
>>> list(graph.node_values.values())[1]
```

Cette manière est **plus stable** car elle ne dépend pas de l'identité exacte de l'objet. L'ordre des valeurs suit la séquence dans laquelle les nœuds ont été exécutés lors du dernier `graph.execute_flow()`. L'index `[1]` correspond au deuxième nœud de cette séquence.

> **💡 Note :** Si vous voulez voir l'index de chaque nœud dans l'ordre d'exécution, vous pouvez utiliser :
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ Attention :** `graph.node_values` ne renvoie pas toujours un tableau directement. Pour les nœuds processeurs (FFT, filtres, etc.) il renvoie un **dictionnaire** où chaque clé est un port de sortie. Pour les nœuds source, il renvoie un tuple `(x, y)`.

#### Alternative : voir les données de tous les nœuds en une ligne
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### Accéder à l'axe Y d'un nœud source

Les nœuds générateurs (SignalSourceNode, FileInputNode, etc.) renvoient un tuple `(temps, signal)` lors de l'exécution. Pour obtenir uniquement l'axe Y :

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**Exemple de sortie :**
```
Scalar NumPy (float64): 1.0
```

#### Alternative : obtenir l'axe X (temps)

```python
>>> x = result[0]
>>> x[:5]
```

### Affectations Python : un détail vital

En Python, les affectations (`=`) sont des **instructions**, pas des expressions. La console n'imprime rien après `x, y = ...` car il n'y a pas de valeur de retour.

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

Pour vérifier que cela a fonctionné, évaluez la variable à la ligne suivante :

```python
>>> x
>>> y.shape
```

Ou utilisez `;` pour chaîner une expression sur la même ligne :

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

Ou utilisez `print()` explicitement :

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. Dessiner sur le graphique principal

### Effacer le graphique

```python
>>> app.plot_widget.clear_plot()
```

#### Alternative : effacer et redessiner immédiatement

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### Dessiner un signal arbitraire depuis la console

Vous pouvez créer des tableaux avec NumPy et les envoyer directement au widget de tracé, sans passer par aucun nœud.

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### Alternative : dessiner une somme de sinus

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### Dessiner le résultat d'un nœud source

Comme le nœud source renvoie `(x, y)`, vous pouvez décompresser directement :

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### Dessiner le résultat d'un nœud FFT (ports multiples)

Les nœuds avec plusieurs sorties (FFT, analyse temps-fréquence, etc.) ne renvoient pas un tuple simple. Ils renvoient un `dict` où chaque clé est un port de sortie.

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**Exemple de sortie :**
```
dict_keys(['output', 'magnitude', 'phase'])
```

Notez que `'output'` peut être `None` si le nœud n'a pas de port générique. Les sorties utiles sont `'magnitude'` et `'phase'`, qui sont à leur tour des tuples `(fréquences, valeurs)` :

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### Alternative : dessiner la phase au lieu de la magnitude

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### Alternative : superposer deux signaux

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # un autre nœud
>>> app.plot_widget.plot_waveform(x2, y2)  # se superpose
```

> **💡 Note :** Si vous essayez de faire `x, y = result` avec un dict, Python lèvera `ValueError: too many values to unpack`. Inspectez toujours avec `type(result)` et `result.keys()` avant de décompresser.

---

## 5. Modifier l'application à la volée

### Mettre à jour le tableau de paramètres latéral

Si vous modifiez un paramètre par code et voulez que le tableau latéral reflète le changement :

```python
>>> app.workspace_table.populate()
```

Cette méthode ne reçoit pas d'arguments. Elle rafraîchit le tableau avec les valeurs actuelles du nœud sélectionné.

#### Alternative : forcer la sélection d'un autre nœud et rafraîchir

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### Ajouter un nœud depuis la barre d'outils

```python
>>> app.add_node('SignalSourceNode')
```

C'est équivalent à appuyer sur le bouton "+" de la barre d'outils. Le nœud est placé à une position par défaut sur le canevas.

### Changer le titre de la fenêtre

`setWindowTitle` est la méthode native de Qt. Elle fonctionne, mais gardez à l'esprit que l'application peut avoir un timer ou un événement qui appelle `update_title()` et l'écrase automatiquement.

```python
>>> app.setWindowTitle('Mon laboratoire de signaux')
>>> app.windowTitle()
```

Pour restaurer le titre "officiel" que l'application calcule depuis son état interne (nom du projet, fichier, etc.) :

```python
>>> app.update_title()
```

#### Alternative : titre avec nom du projet

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. Navigation et productivité dans la console

La console n'est pas un simple `print()`. Elle a un historique, un autocomplétion et des blocs multilignes.

| Touche / Commande         | Action                                                         |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | Naviguer dans l'historique des commandes exécutées             |
| `Tab`                     | Autocompléter variables, attributs et méthodes du namespace    |
| `Ctrl + L`                | Effacer toute la console (supprime le texte, pas l'état Python) |
| `if`, `for`, `def`, `class` | Le prompt passe de `>>>` à `...` pour les blocs multilignes   |
| `Ctrl+C` (sur sélection)  | Copier le texte de la console                                  |
| `Ctrl+A`                  | Sélectionner tout le contenu                                   |

> **💡 Note :** L'autocomplétion utilise `rlcompleter` et reconnaît tout le namespace injecté (`app`, `graph`, `selected_node`) plus toute variable que vous définissez dans la session.

---

## 7. Recettes avancées

### Changer un paramètre interne d'un nœud

Les paramètres des nœuds ne sont pas des attributs plats. Ils sont imbriqués dans le dictionnaire `params`, qui a à son tour des sous-sections comme `'preset'`, `'formula'` ou `'advanced'`. Ne faites jamais `node.amplitude = 3.0` ; cela crée un nouvel attribut sur l'objet mais ne modifie pas le vrai paramètre.

#### Cas A : modifier un preset (sinus, carrée, etc.)

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Cas B : utiliser une formule personnalisée

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

L'expression utilise `t` comme variable de temps. Les valeurs dans `vars` sont les symboles que vous pouvez référencer dans la formule. Si vous omettez `vars`, le nœud utilisera des valeurs par défaut et la formule peut ne pas refléter le changement.

#### Cas C : changer des paramètres avancés (taux d'échantillonnage, durée)

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### Cas D : changer le paramètre d'un nœud non générateur (ex. FFT)

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 Note :** `getattr(obj, '_generate_signal', lambda: None)()` est un motif sûr : si la méthode existe (nœuds générateurs), elle l'appelle ; sinon, elle ne fait rien et ne lève pas d'erreur. Pour les nœuds processeurs, seul `graph.execute_flow()` suffit.

### Lister toutes les catégories de nœuds disponibles

L'import charge le catalogue, mais ne l'affiche pas automatiquement. Rappelez-vous qu'en Python un import réussi n'imprime rien ; vous devez évaluer l'objet.

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

Pour voir un résumé lisible :

```python
>>> for cat, nodes in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(nodes)} nœuds")
```

#### Alternative : lister les noms de nœuds par catégorie

```python
>>> {cat: [n.__name__ for n in nodes] for cat, nodes in NODE_CATEGORIES.items()}
```

### Voir l'aide de n'importe quelle méthode

```python
>>> help(graph.connect_nodes)
```

La docstring apparaît directement dans la console. C'est utile pour découvrir quels arguments attend une méthode sans ouvrir le code source.

#### Alternative : voir les attributs filtrés

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. Que faire si quelque chose ne va pas ?

- **Erreur en rouge dans la console :** le traceback complet s'affiche. L'application ne se ferme pas ; vous pouvez corriger la commande et réessayer.
- **L'interface se fige :** vous avez probablement écrit une boucle infinie. La console s'exécute dans un thread séparé, mais si la boucle affecte le thread GUI, redémarrez l'application.
- **`None` inattendu :** si un nœud renvoie `None` au lieu de données, vérifiez qu'il est connecté en amont (`graph.connections`) et que le flux a été exécuté (`graph.execute_flow()`).
- **`ValueError: too many values to unpack` :** vous essayez de décompresser un dict comme s'il s'agissait d'un tuple. Utilisez `result.keys()` d'abord.
- **`ValueError: not enough values to unpack` :** vous attendez 2 valeurs mais le nœud en renvoie 1 (dict) ou 3 (spectrogramme). Inspectez avec `type(result)` avant de décompresser.
- **`AttributeError` :** l'objet n'a pas cet attribut. Utilisez `dir(obj)` ou `[a for a in dir(obj) if 'mot' in a.lower()]` pour découvrir le nom correct.
- **Rien ne se passe lors de l'exécution :** vérifiez qu'il y a au moins un nœud source connecté à la chaîne et que `graph.execute_flow()` a été appelé. Les nœuds processeurs ne génèrent pas de données seuls.
- **Le graphique ne change pas :** assurez-vous d'appeler `graph.execute_flow()` après avoir modifié des paramètres. Se changer `params` ne recalcule pas automatiquement.

---

© 2026 FloWorks — Laboratoire de Signaux
