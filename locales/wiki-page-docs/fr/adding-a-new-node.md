---
title: Guide pour Ajouter un Nouveau Nœud à FloWorks
description: Tutoriel pas à pas pour créer, enregistrer et intégrer des nœuds personnalisés dans le moteur de flux de FloWorks.
---

# 📘 Guide pour Développeurs : Comment Ajouter un Nouveau Nœud à FloWorks

Ce guide décrit le processus complet pour créer un nouveau type de nœud dans FloWorks, en s'assurant qu'il s'intègre correctement avec le moteur de flux, l'interface utilisateur, les thèmes visuels et le système d'internationalisation.

---

## 📋 Table des Matières
- [📘 Guide pour Développeurs : Comment Ajouter un Nouveau Nœud à FloWorks](#-guide-pour-développeurs--comment-ajouter-un-nouveau-nœud-à-floworks)
  - [📋 Table des Matières](#-table-des-matières)
  - [1. Introduction à l'Architecture](#1-introduction-à-larchitecture)
  - [2. Utilisation du Modèle `template_node.py`](#2-utilisation-du-modèle-template_nodepy)
  - [3. Pas à Pas : Création d'un Nœud Personnalisé](#3-pas-à-pas--création-dun-nœud-personnalisé)
    - [3.1. Copier et Renommer le Modèle](#31-copier-et-renommer-le-modèle)
    - [3.2. Définir les Ports et les Étiquettes](#32-définir-les-ports-et-les-étiquettes)
    - [3.3. Implémenter la Logique de Traitement](#33-implémenter-la-logique-de-traitement)
    - [3.4. Personnaliser l'Apparence (Optionnel)](#34-personnaliser-lapparence-optionnel)
    - [3.5. Ajouter des Paramètres Configurables (Optionnel)](#35-ajouter-des-paramètres-configurables-optionnel)
    - [3.6. Rendre le Nœud Sérialisable (enregistrer / charger les configurations)](#36-rendre-le-nœud-sérialisable-enregistrer--charger-les-configurations)
  - [4. Intégration dans le Système](#4-intégration-dans-le-système)
  - [5. Internationalisation (i18n)](#5-internationalisation-i18n)
  - [6. Thèmes Visuels](#6-thèmes-visuels)
  - [7. Liste de Vérification et Résolution de Problèmes](#7-liste-de-vérification-et-résolution-de-problèmes)
    - [✅ Liste de Vérification](#-liste-de-vérification)
    - [🐛 Problèmes Courants](#-problèmes-courants)
  - [8. Conclusion](#8-conclusion)

---

## 1. Introduction à l'Architecture

FloWorks est construit sur PySide6 et utilise un modèle de nœuds connectables qui représentent un flux de traitement de signaux.

---

## 2. Utilisation du Modèle `template_node.py`

Pour faciliter la création de nouveaux nœuds, le fichier `nodes/template_node.py` est fourni. Ce modèle inclut :
- Support complet pour l'internationalisation (connexion à `languageChanged`, méthode `update_language`).
- Support complet pour les thèmes (méthode `update_theme`).
- Aide intégrée avec un format HTML en trois sections.
- Gestion de multiples ports d'entrée/sortie configurables.
- Sorties multiples avec `get_output_for_port`.
- Visualisation dans le graphique via `get_display_signal`.
- Menu contextuel traduisible.

Il est recommandé de toujours partir de ce modèle lors du développement d'un nouveau nœud.

---

## 3. Pas à Pas : Création d'un Nœud Personnalisé

### 3.1. Copier et Renommer le Modèle
1. Copiez `nodes/template_node.py` avec le nom de votre nouveau nœud, par exemple `nodes/mon_noeud.py`.
2. Renommez la classe de `TemplateNode` en quelque chose de descriptif, par ex. `MonNoeudNode`.
3. Ajustez les imports si nécessaire.

### 3.2. Définir les Ports et les Étiquettes
!!! warning "Important : Correspondance des Noms"
    Les noms de port dans `PORTS`, `PORT_LABELS` et les clés du dictionnaire renvoyé par `execute_program` doivent être **exactement identiques** (majuscules/minuscules comprises). Le modèle inclut désormais un mappage d'alias (`'data_in'` → premier port gauche) pour plus de robustesse.

Modifiez le dictionnaire `PORTS` en haut du fichier. Chaque entrée a le format :
```python
"nom_port": ("côté", fraction)
```
- **Côtés possibles :** `"left"`, `"right"`, `"top"`, `"bottom"`.
- **Fraction :** valeur entre `0.0` et `1.0` indiquant la position le long du côté.

**Exemple pour un nœud avec une entrée et deux sorties :**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
Le dictionnaire `PORT_LABELS` contient le texte qui apparaîtra à côté de chaque port. Il est recommandé d'utiliser des clés de traduction plutôt que du texte fixe (voir la section Internationalisation).

### 3.3. Implémenter la Logique de Traitement
La méthode clé est `execute_program(self, input_data)`. Cette méthode est invoquée par le moteur de flux lorsque le nœud reçoit des données.

**`input_data` peut être :**
- `None` s'il n'y a pas d'entrée.
- Un tuple `(x, y)` pour les signaux temporels.
- Un tableau 1D.
- Un dictionnaire `{nom_port: données}` dans les nœuds avec entrées multiples.

**Valeur de retour :**
- Pour les nœuds avec une seule sortie, renvoyez directement les données (ex. tuple `(x, y)`).
- Pour les nœuds avec plusieurs sorties, renvoyez un dictionnaire dont les clés correspondent aux noms des ports de sortie définis dans `PORTS`.

```python
def execute_program(self, input_data):
    # Traiter input_data et générer les résultats
    resultat_magnitude = (freq, mag)
    resultat_phase = (freq, phase)
    return {
        "magnitude": resultat_magnitude,
        "phase": resultat_phase
    }
```

!!! tip "Note sur les noms de port génériques"
    Le moteur de flux peut occasionnellement passer un dictionnaire avec des clés comme `'data_in'` au lieu du vrai nom de port (surtout si l'utilisateur n'a pas cliqué exactement sur le cercle). Le modèle inclut déjà du code pour gérer ce cas :
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    Cela évite que le nœud échoue à cause d'une erreur de connexion imprécise.

Le modèle inclut déjà un exemple commenté. De plus, il implémente `get_output_for_port(self, port_name)` pour que le moteur puisse router chaque sortie :
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. Personnaliser l'Apparence (Optionnel)
La méthode `paint()` dessine le fond, le titre, l'état et tout texte supplémentaire. Vous pouvez modifier :
- Les couleurs (elles sont mises à jour automatiquement avec `update_theme`).
- Le texte d'état (en utilisant l'attribut `self._status`).
- Les informations de résumé (ex. pic de magnitude).

Le modèle montre un exemple basique.

### 3.5. Ajouter des Paramètres Configurables (Optionnel)
Si votre nœud nécessite des paramètres ajustables par l'utilisateur (ex. taille de fenêtre, fréquence de coupure), vous pouvez :
1. Ajouter des attributs dans `__init__` (ex. `self.window_size = 512`).
2. Créer une boîte de dialogue de configuration (hérite de `QDialog`).
3. Connecter la boîte de dialogue dans `open_config_dialog()` (méthode déjà présente dans le modèle).
4. Mettre à jour les paramètres depuis la boîte de dialogue et appeler `self.update()`.

### 3.6. Rendre le Nœud Sérialisable (enregistrer / charger les configurations)
Pour que le nœud puisse enregistrer et récupérer ses paramètres lors de copier/coller, annuler/refaire, ou lors de l'utilisation des commandes Enregistrer/Ouvrir du menu Fichier, il doit hériter du mixin de sérialisation et déclarer ses attributs.

1. Importez le mixin dans votre fichier :
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. Modifiez l'héritage de la classe pour l'inclure avant `QGraphicsObject` :
    ```python
    class MonNoeudNode(SerializableMixin, QGraphicsObject):
    ```
3. Définissez la liste `SERIALISABLE` au niveau de la classe, avec les noms des attributs que vous voulez persister. Seuls les types simples (`int`, `float`, `str`, `bool`), les listes, les dictionnaires ou les tableaux NumPy sont pris en charge (ces derniers sont automatiquement stockés comme fichiers `.npy` à l'intérieur du `.sflow`).
    ```python
    class MonNoeudNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frequence', 'amplitude', 'configuration']
    ```
4. Assurez-vous que ces attributs sont initialisés dans `__init__` :
    ```python
    self.frequence = 1000.0
    self.amplitude = 1.0
    self.configuration = {'type': 'sinus', 'phase': 0}
    ```

Avec cela, vous n'avez pas besoin d'écrire de méthodes `serialize`/`deserialize` ; le mixin se charge automatiquement d'enregistrer et de récupérer les valeurs.

Si votre nœud nécessite une logique supplémentaire au chargement (par exemple, reconnecter un instrument matériel), vous pouvez surcharger `deserialize` en appelant d'abord la méthode parente :
```python
def deserialize(self, data):
    super().deserialize(data)   # restaure les attributs de SERIALISABLE
    self._initier_dispositif()
```

---

## 4. Intégration dans le Système

Une fois le fichier du nœud créé, vous n'avez qu'à le coller dans le dossier `nodes` pour qu'il apparaisse dans l'interface et fonctionne avec le reste du système.

---

## 5. Internationalisation (i18n)

Tous les textes visibles doivent être traduisibles via `tr("clé", default="...")`. Le modèle l'implémente déjà. Vous devez ajouter les clés correspondantes dans les fichiers JSON à l'intérieur de `locales/`.

**Structure recommandée :**
```json
{
   "nodes": {
     "mon_noeud": {
       "title": "Mon Nœud",
       "tooltip": "Description de l'infobulle",
       "ports": {
         "input": "Entrée",
         "output1": "Sortie 1",
         "output2": "Sortie 2"
      },
       "status": {
         "no_data": "Pas de données",
         "ready": "Prêt"
      },
       "menu": {
         "show_output": "Afficher la sortie",
         "configure": "Configurer..."
      },
       "help_title": "Aide - Mon Nœud",
       "help_html": "<h3>🎛️ Filter Node</h3>
<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_mon_noeud": "Mon Nœud"
  }
}
```

L'aide HTML suit le format en trois sections commun à tous les nœuds (description spécifique + "Comment penser le système" + "Raccourcis et astuces"). Le modèle inclut déjà la structure dans `get_help_text()`.

---

## 6. Thèmes Visuels

La méthode `update_theme(self, theme)` reçoit un dictionnaire avec les couleurs définies par le thème actuel. Le modèle met à jour automatiquement :
- Fond du nœud (`node_normal_bg`)
- Bordure (`node_selected_border`)
- Couleur du titre et du texte (`node_normal_text`)
- Couleurs des ports (`port_circle`, `port_outline`, `port_inline`, `port_text`)

Assurez-vous que dans `MainWindow` (ou `ThemeUpdater`) `node.update_theme()` soit appelé pour chaque nœud lorsque le thème change.

---

## 7. Liste de Vérification et Résolution de Problèmes

### ✅ Liste de Vérification
- [ ] Le nœud se crée correctement depuis la barre d'outils.
- [ ] Les ports s'affichent aux positions attendues et sont détectables pour les connexions (`Ctrl+clic`).
- [ ] Lors de la réception de données d'entrée, `execute_program` est appelé et le signal est traité.
- [ ] Les sorties se propagent correctement aux nœuds connectés.
- [ ] Le menu contextuel permet de changer le canal d'affichage (s'il y a plusieurs sorties).
- [ ] En cliquant sur le nœud, le signal sélectionné est tracé dans le widget de graphique.
- [ ] Le double clic ouvre l'aide avec le format approprié.
- [ ] La langue change correctement (textes de titre, ports, menus).
- [ ] Le thème change correctement (couleurs du nœud et des ports).
- [ ] Copier/coller fonctionne sans erreurs.

!!! tip "Connexion précise des ports"
    Lors de la connexion des nœuds, assurez-vous de cliquer exactement sur le cercle du port cible. Si vous cliquez sur le corps du nœud, le système utilisera un nom générique (`'data_in'`). Le modèle tolère désormais ces noms, mais c'est une bonne pratique de se connecter directement au cercle pour garantir le routage correct des sorties multiples.

### 🐛 Problèmes Courants

| Symptôme | Cause Possible | Solution |
|---------|---------------|----------|
| La flèche de connexion ne s'ancre pas au port. | Le cercle du port n'a pas `setData(0, port_name)` ou `get_port_scene_pos` n'est pas implémenté. | Vérifier que dans `_create_ports` on fasse `circle.setData(0, port_name)` et que `get_port_scene_pos` utilise ce nom. |
| Les sorties n'arrivent pas aux nœuds connectés. | `execute_program` ne renvoie pas un dictionnaire (pour plusieurs sorties) ou `get_output_for_port` n'est pas implémenté. | S'assurer que `execute_program` renvoie `{nom_port: données}` et que `get_output_for_port` renvoie la valeur correspondante. |
| En cliquant sur le nœud rien n'est tracé. | `get_display_signal` ne renvoie pas un tuple `(x, y)` valide ou `display_channel` ne correspond pas à une sortie existante. | Vérifier que `get_display_signal` utilise le canal sélectionné et que les données sont des tableaux NumPy. |
| Les textes ne se mettent pas à jour au changement de langue. | Le signal `languageChanged` n'a pas été connecté ou `update_language` ne met pas à jour les éléments. | Vérifier la connexion dans `__init__` : `language_manager.languageChanged.connect(self.update_language)`. |
| Le thème ne s'applique pas. | `update_theme` n'est pas appelé lors de la création du nœud ou au changement de thème. | Dans `MainWindow`, après avoir créé le nœud, invoquer `node.update_theme(self.theme_manager.current_theme())`. |
| La flèche pointe au centre du nœud. | On a cliqué sur le corps au lieu du cercle, ou le nom ne correspond pas à `PORTS`. | Cliquez directement sur le cercle. Vérifiez que `get_port_scene_pos` ait le mappage d'alias. |
| `NameError: name 'self' is not defined` à l'importation. | Des attributs d'instance ont été déclarés en dehors de `__init__`. | Tous les attributs comme `self.mon_parametre` doivent être définis dans `__init__`. |
| Les paramètres se perdent lors de la copie/ouverture du `.sflow`. | Le nœud n'hérite pas de `SerializableMixin` ou n'a pas défini `SERIALISABLE`. | Implémenter l'étape 3.6 de ce guide. |

---

## 8. Conclusion

En suivant ce guide et en utilisant le modèle `template_node.py`, vous pourrez ajouter de nouveaux nœuds à FloWorks de manière efficace et cohérente avec le reste du système. N'oubliez pas de maintenir toujours la compatibilité avec i18n et les thèmes pour une expérience utilisateur professionnelle.

N'hésitez pas à contribuer avec vos propres nœuds !
