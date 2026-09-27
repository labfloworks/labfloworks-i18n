---
title: Premiers pas avec FloWorks
description: Guide rapide pour configurer l'environnement, exécuter votre premier flux et accéder à la version portable.
---

# 🚀 Premiers pas avec FloWorks

Ce guide vous mènera de zéro jusqu'à ce que votre premier flux de traitement de signaux soit en cours d'exécution. FloWorks est une application de diagrammes de flux pour signaux, construite avec Python et PySide6, qui supporte le matériel réel (VISA/SCPI), la simulation intégrée, le scripting avancé et le changement de langue à chaud.

---

## 🌊 Votre premier flux d'exemple

Créons un flux simple : générer un signal sinusoïdal et le visualiser en temps réel.

1. **Ajouter des nœuds**
   Dans la barre d'outils supérieure, sélectionnez `Source` → sélectionnez `Générateur de signaux avancé`. Ensuite, depuis `Traitement` → sélectionnez, par exemple, `Spectral`.
2. **Connecter**
   Faites `Ctrl+Clic` sur le port de sortie (`droite`) du générateur. Puis, `Clic` sur le port d'entrée (`gauche`) de l'oscilloscope. Ou simplement cliquez sur le port de sortie et faites glisser (en maintenant le clic) jusqu'au port d'entrée du nœud suivant.
3. **Configurer (optionnel)**
   Cliquez sur un nœud, et un éditeur avec les paramètres du nœud sélectionné apparaîtra dans le panneau latéral gauche pour ajuster les conditions de fonctionnement. En bas se trouve un graphique de visualisation qui représente visuellement les données générées ou acquises par les nœuds.
4. **Exécuter**
   Appuyez sur `F5` ou le bouton ▶ sur la barre d'outils. Le moteur topologique calculera l'ordre d'exécution, traitera les données et vous verrez l'onde sur le panneau graphique. Le connecteur s'animera en indiquant le flux actif !


---

## 🧠 Comprendre les ports : catégories par couleur

Dans FloWorks, chaque port appartient à une **catégorie fonctionnelle** identifiée par une couleur. Les connexions valides se font **toujours entre ports de la même couleur** : une sortie d'une catégorie se connecte uniquement à une entrée de la même catégorie. De plus, la ligne connectrice adopte automatiquement la couleur des ports qu'elle joint, facilitant la lecture visuelle.

| Type | Couleur | But | Exemple typique |
|------|-------|-----------|----------------|
| `control` | Blanc  | Flux de contrôle / activation. | Signal de démarrage vers un nœud d'acquisition. |
| `exec` | Gris | Exécution d'opérations ou d'étapes. | Déclenchement d'une fonction ou callback. |
| `data` | Vert  | Données génériques / signaux numériques. | Sortie d'un générateur ou capteur. |
| `int` | Bleu  | Nombres entiers. | Index, taille de buffer, ID. |
| `float` | Cyan  | Nombres à virgule flottante. | Amplitude, fréquence, seuil. |
| `string` | Violet  | Chaînes de texte. | Nom de fichier, étiquette. |
| `bool` | Rose  | Valeurs booléennes (`True`/`False`). | Drapeau d'état, habilitation. |
| `array` | Bleu foncé  | Tableaux / vecteurs. | Signal multicanal, liste d'échantillons. |
| `trigger` | Orange  | Déclencheurs / événements discrets. | Impulsion de synchronisation, front. |

**Règle d'or :**

- Seuls les ports de la **même couleur exacte** se connectent (sortie ↔ entrée de la même catégorie).
- Le système empêche les connexions invalides et met en évidence visuellement les ports compatibles lors du glisser-déposer.
- La ligne connectrice prend la couleur des ports connectés ; ainsi chaque route s'identifie d'un coup d'œil.

**Philosophie FloWorks :**
Les ports de données **préservent la dimensionalité** des tableaux. Un aplanissement automatique n'est jamais appliqué : si une matrice entre, une matrice sort, maintenant l'intégrité de vos signaux multidimensionnels.

![FloWorks](assets/tipos_de_puertos.PNG)

---

## 🖱️ Navigation sur le Canevas

Maîtrisez l'espace de travail avec ces gestes :

| Action | Comment le faire |
|--------|--------------|
| **Zoom** | Roulette de la souris ou `Ctrl + roulette` |
| **Pan (défilement)** | Maintenez `Espace` et faites glisser, ou utilisez le bouton central de la souris |
| **Sélectionner un nœud** | Clic gauche sur le nœud |
| **Sélection multiple** | Faites glisser un rectangle avec clic gauche, ou `Ctrl + clic` sur plusieurs nœuds |
| **Déplacer la sélection** | Faites glisser l'un des nœuds sélectionnés |
| **Ouvrir la configuration** | Double-clic sur un nœud |

**Conseil :** Le panneau gauche se met à jour automatiquement avec la configuration du nœud sélectionné, sans nécessité d'ouvrir des fenêtres supplémentaires.

---

## ⚡ Raccourcis clavier et mouvements avancés

Ces raccourcis transforment un utilisateur normal en **utilisateur avancé** :

| Raccourci | Action |
|-------|--------|
| `F5` | Exécuter le flux |
| `Ctrl + S` | Enregistrer le projet (`.sflow`) |
| `Ctrl + Clic` | Connecter des nœuds (clic sur port de sortie → clic sur port d'entrée) |
| `Ctrl + C` / `Ctrl + V` | Copier / coller les nœuds sélectionnés |
| `Ctrl + Z` / `Ctrl + Y` | Annuler / refaire |
| `Ctrl + Maj + L` | Organiser automatiquement les nœuds sur le canevas |
| `Suppr` | Supprimer les nœuds sélectionnés |
| `Ctrl + A` | Sélectionner tous les nœuds |

**Mouvements avancés :**

- **Dupliquer un flux :** sélectionnez un groupe de nœuds, `Ctrl + C`, `Ctrl + V` et faites glisser la copie vers une autre zone.
- **Nettoyer la grille :** utilisez `Ctrl + Maj + L` pour ordonner tout le canevas en une seule commande.
- **Connexion rapide :** `Ctrl + Clic` sur un port de sortie puis clic normal sur le port d'entrée ; FloWorks dessine la connexion automatiquement.

---

## 🎨 Personnalisation de l'environnement

FloWorks s'adapte à vous, et non l'inverse.

### Changement de thème à chaud
Depuis la barre supérieure, menu **Affichage → Thème**, choisissez entre clair, sombre ou autres. L'interface change **instantanément**, sans redémarrer ni perdre votre flux de travail.

### Taille de police
Dans **Affichage → Taille de police** sélectionnez une valeur prédéfinie ou une valeur personnalisée. Toute l'interface s'ajuste immédiatement.

### Langue
Dans **Affichage → Langue** sélectionnez la langue désirée. FloWorks supporte le **changement à chaud** : les menus, boutons et messages sont traduits sans redémarrer l'application.

---

## ❗ Résolution de problèmes courants

| Problème | Cause possible | Solution |
|----------|---------------|----------|
| Le flux ne s'exécute pas | Il y a des nœuds non configurés ou des connexions rompues | Vérifiez que tous les nœuds ont des paramètres valides et que les connexions sont entre ports compatibles |
| Le graphique ne se met pas à jour | Le flux est en pause ou aucune donnée ne circule | Assurez-vous d'avoir appuyé sur `F5` ou ▶, et que les nœuds source génèrent des données |
| Je ne peux pas connecter deux nœuds | Les ports sont de type différent | Vérifiez que les deux ports soient de **données** ou tous deux de **contrôle** |
| Le programme ralentit avec de grands flux | Trop de nœuds ou de graphiques en temps réel | Fermez les panneaux d'analyse non utilisés ou réduisez la fréquence d'échantillonnage des nœuds source |
| Le thème ne change pas | Certains widgets peuvent ne pas être enregistrés | Redémarrez l'application et réessayez (ce sera résolu dans les versions futures) |

---

## 🧪 Exemples pratiques rapides

Outre le flux sinusoïdal initial, essayez ces mini-projets pour maîtriser FloWorks :

| Exemple | Nœuds impliqués | Résultat attendu |
|---------|-------------------|--------------------|
| **Filtre passe-bas** | Générateur → Filtre → Visualiseur de graphiques | Vous verrez le signal filtré |
| **Acquisition simulée** | Générateur → Analyseur de THD | Valeur de la distorsion harmonique du signal |
| **Contrôle manuel** | Générateur → Inspecteur de données | Tableau avec les valeurs du signal envoyé par le générateur |
| **Comparaison de signaux** | Deux générateurs → Additionneur → Visualiseur de graphiques | Le résultat de l'opération (addition, soustraction, multiplication ou division) de deux ondes en un seul graphique |

Chacun de ces flux peut être monté en moins d'une minute, démontrant l'agilité de FloWorks face au code traditionnel.

---

## 📚 Et ensuite ?

| Ressource | Description |
|---------|-------------|
| [🗺️ Guide d'Anatomie de l'Interface Principale](interface-anatomy.md) | Comprendre l'architecture et la philosophie de l'interface graphique |
| [🗺️ Carte du Code et Architecture](philosophy.md) | Structure complète, managers, contrats et DPI-Awareness. |
| [🧩 Référence Technique des Nœuds](node-reference.md) | Catalogue, `ScriptNode`, multicanal et comment étendre le système. |
| [🌐 Guide d'Internationalisation](translation-guide.md) | Ajouter des langues, valider le JSON et gérer les clés `tr()`. |
| [📦 Guide de Build Portable](guia-ejecutable-portable.md) | PyInstaller, hooks, `--onefile`, résolution d'erreurs et signature numérique. |

---

!!! warning "Notes de compatibilité et d'utilisation"
    1. **Version de Python :** Vous pouvez utiliser 3.9+, et des systèmes 64 bits.
    2. **Pare-feu Windows :** Si vous utilisez du matériel réel (oscilloscope VISA/SCPI), autorisez `FloWorks.exe` dans le pare-feu. L'app affiche un dialogue personnalisé si la connexion est bloquée (le dialogue du OS n'apparaît pas en mode `--windowed`).
    3. **Raccourcis clés :** `F5` (exécuter), `Ctrl+S` (enregistrer `.sflow`), `Ctrl+Clic` (connecter), `Espace+clic` (pan libre), `Ctrl+Maj+L` (auto-layout).
    4. **Préservation des données :** Le moteur n'applique **jamais** `flatten()` aux tableaux. Travaillez avec des copies locales si vous devez vectoriser.
