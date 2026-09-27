---
title: Architecture de FloWorks
description: Vue d'ensemble des composants et du fonctionnement interne pour l'utilisateur final
---

# Architecture de FloWorks – Vue utilisateur

FloWorks est une application de bureau qui vous permet de construire des chaînes de traitement de signaux à l'aide de diagrammes de flux. Vous connectez des blocs (nœuds) sur un canevas interactif et voyez les résultats en temps réel. Pour rendre cela possible, l'application est organisée en plusieurs modules qui travaillent ensemble. Ci-dessous, une explication non technique de ce que fait chaque partie et comment elles se relationnent.

---

## Structure générale

L'application se compose des domaines fonctionnels suivants :

| Domaine | Que fait-il ? |
|------|------------|
| **Démarrage et fenêtre principale** | Lance le programme, affiche la fenêtre, les menus et coordonne toutes les actions de l'utilisateur. |
| **Moteur d'exécution** | Calcule l'ordre dans lequel les nœuds doivent s'exécuter, détecte les dépendances et les boucles, et transmet les données d'un nœud à l'autre. |
| **Scène et diagramme** | Gère le canevas où vous placez les nœuds, les connexions entre eux, les notes autocollantes et les actions annuler/refaire. |
| **Nœuds et traitement** | Contient tous les types de blocs que vous pouvez utiliser : sources de signaux, opérations mathématiques, scripts personnalisés, exportation de graphiques, etc. |
| **Connecteurs visuels** | Dessine les lignes qui joignent les nœuds (courbes douces ou orthogonales), les anime pour montrer le flux de données et évite qu'ils se chevauchent. |
| **Interface utilisateur** | Inclut la vue du diagramme (zoom, déplacement), la barre d'outils, la table des paramètres, les panneaux d'analyse (statistiques, curseurs) et les boîtes de dialogue de configuration. |
| **Support du matériel réel** | Permet la communication avec des instruments de laboratoire (oscilloscopes, générateurs, multimètres LCR) pour capturer ou générer des signaux réels. |
| **Exportation de graphiques** | Génère des images de haute qualité (PNG, PDF, SVG) avec une personnalisation visuelle complète. |
| **Thèmes et apparence** | Change l'aspect de toute l'application (sombre, clair, contraste élevé) et permet d'ajuster la taille de la police. |
| **Langues** | Traduit toute l'interface en plusieurs langues et permet de changer de langue instantanément. |
| **Gestion de projets** | Enregistre et ouvre des fichiers `.sflow` avec tout le diagramme, incluant configurations, scripts et résultats. |
| **Tests et diagnostic** | Outils internes pour vérifier que tout fonctionne correctement (non visibles pour l'utilisateur final). |

---

## Comment ça marche à l'intérieur

### Démarrage et fenêtre principale
En ouvrant FloWorks, l'environnement graphique est configuré, la densité de pixels de votre écran est détectée (pour que tout soit net sur les écrans 4K ou normaux) et la fenêtre principale est affichée. Cette fenêtre centralise tous les éléments : la zone de dessin, les menus, la barre d'outils et les panneaux latéraux.

### Moteur de flux
Lorsque vous appuyez sur "Exécuter" (ou sur F5), un moteur interne parcourt tous les nœuds dans le bon ordre, en respectant les connexions. Il sait quels nœuds dépendent des autres et évite les boucles infinies. Il supporte qu'un nœud reçoive plusieurs entrées nommées et produise plusieurs sorties. Les données voyagent entre les nœuds sans perdre leur structure originale.

### Scène du diagramme
Le canevas où vous construisez vos diagrammes est une scène intelligente :

- Permet d'ajouter, déplacer, connecter et sélectionner des nœuds.
- Supporte des annulations et refaires illimités pour toute action.
- Inclut des notes autocollantes redimensionnables que vous pouvez placer librement et qui sont enregistrées avec le projet.
- Dispose d'un organisateur automatique qui repositionne les nœuds proprement (avec Ctrl+Shift+L).
- Lors de l'enregistrement, tout le diagramme est empaqueté dans un fichier `.sflow` contenant les descriptions des nœuds, les connexions, les notes et les données numériques associées.

### Connecteurs
Les lignes qui joignent les nœuds sont dessinées comme des courbes douces ou des trajectoires orthogonales. Une douce animation de points ou de tirets indique la direction du flux. Un gestionnaire de voies évite que plusieurs connexions entre les mêmes nœuds ne s'empilent ; il les sépare automatiquement pour que tout reste lisible.

### Types de nœuds
Les nœuds sont les pièces fondamentales. Ils sont regroupés en trois catégories :

- **Sources** – Génèrent des signaux. Elles peuvent simuler des ondes (sinusoïdale, carrée, etc.) ou lire des données réelles depuis un oscilloscope ou un multimètre connecté. Elles supportent plusieurs canaux simultanés (par exemple, impédance et phase d'un LCR).
- **Traitement** – Transforment les données. Ils incluent des opérations arithmétiques (addition, soustraction, multiplication, division), des décisions conditionnelles (branchement Oui/Non) et un puissant nœud de script qui vous permet d'écrire votre propre code Python avec des aides visuelles.
- **Puits** – Affichent ou exportent les résultats. Le plus commun est le visualiseur graphique (oscilloscope virtuel), mais il existe aussi un exportateur de graphiques de qualité professionnelle.

Chaque nœud a des ports d'entrée (gauche/haut) et de sortie (droite/bas). Lorsque vous connectez un port de sortie à un port d'entrée, le signal circule entre eux.

#### Nœud de script avancé
Le nœud de script mérite une mention spéciale. Il est destiné aux utilisateurs avancés qui veulent ajouter leur propre traitement sans quitter FloWorks. Il offre :

- Un éditeur avec coloration syntaxique, autocomplétion et numérotation des lignes.
- La possibilité de définir des paramètres éditables depuis le panneau du nœud sans toucher au code (par exemple, une valeur numérique utilisée ensuite dans le script).
- Des ports d'entrée et de sortie dynamiques : en ajoutant des commentaires spéciaux dans le script, vous pouvez créer de nouveaux connecteurs.
- Une mémoire persistante : une variable spéciale (`persist`) qui conserve sa valeur entre les exécutions, utile pour des accumulateurs ou des machines à états.
- Des modèles de scripts déjà préparés et la possibilité d'enregistrer les vôtres.
- Un système d'aide intégré et une console qui affiche les erreurs d'exécution.

### Interface utilisateur
Outre le canevas, l'interface comprend :

- Une **barre d'outils** avec tous les nœuds organisés par catégorie, des menus de langue, thème et taille de police, et un accès au visionneur de journaux.
- Une **table de paramètres** qui affiche les informations des nœuds sélectionnés et met en évidence d'éventuelles incompatibilités (comme essayer d'opérer sur des signaux de longueurs différentes).
- Des **panneaux d'analyse** ancrables : statistiques (maximum, minimum, efficace), curseurs A/B pour mesurer des différences, et un réticule avec marqueur de pic.
- Une **boîte de dialogue de bienvenue** qui s'adapte à la résolution de votre écran et vous propose des options initiales.

### Connexion avec des instruments réels
Si vous disposez de matériel compatible (oscilloscopes Siglent SDS, multimètres LCR, générateurs SDG), FloWorks peut communiquer avec eux via le protocole standard VISA/SCPI. La configuration se fait depuis des panneaux spécifiques à l'intérieur de l'application. Lorsque vous capturez un signal multicanal (par exemple, magnitude et phase d'un LCR), le nœud source empaquete tous les canaux et vous pouvez choisir lequel visualiser avec un simple menu contextuel.

### Exportation de graphiques professionnelle
Le nœud exportateur de graphiques vous permet de générer des images prêtes pour des rapports ou des publications. Un double-clic ouvre une boîte de dialogue avec plusieurs options : vous pouvez personnaliser les couleurs, les types de ligne, les étiquettes, les échelles, choisir entre les formats PNG, PDF ou SVG, et enregistrer vos préférences comme profils réutilisables.

### Personnalisation visuelle
FloWorks inclut plusieurs thèmes (sombre, clair, contraste élevé) qui changent l'apparence de toute l'interface instantanément, sans redémarrer. De plus, vous pouvez ajuster la taille de police globale depuis le menu (Information → Taille de police) et tous les éléments sont redimensionnés en conséquence, y compris les textes à l'intérieur des nœuds, les notes autocollantes et les graphiques.

### Système de langues
L'application détecte automatiquement la langue de votre système lors du premier lancement et enregistre votre préférence. Vous pouvez changer de langue à tout moment depuis le menu ; tous les textes, menus et aides sont mis à jour à la volée.

### Projets et fichiers `.sflow`
Tout votre travail est enregistré dans un seul fichier avec l'extension `.sflow`. Ce fichier contient le diagramme complet : nœuds, connexions, notes, configurations, scripts et données numériques générées. Vous pouvez le partager avec d'autres utilisateurs ; en l'ouvrant sur un autre ordinateur, les notes et les nœuds sont automatiquement remis à l'échelle pour s'adapter à la densité de pixels de cet écran.

---

## Flux de travail typiques

1. **Créer un diagramme simple**
   Sélectionnez un nœud source (p. ex., Générateur) et un nœud Visualiseur depuis la barre d'outils.
   Connectez la sortie du générateur à l'entrée du visualiseur (Ctrl+clic sur le port de sortie, puis clic sur celui d'entrée).
   Appuyez sur F5 pour exécuter. Vous verrez le signal sur le graphique.

2. **Utiliser un script personnalisé**
   Ajoutez un nœud Script.
   Écrivez votre code Python dans l'éditeur ; vous pouvez définir des paramètres éditables et des ports supplémentaires.
   Connectez ses entrées et sorties comme n'importe quel autre nœud.
   Exécutez le flux ; le script est traité avec vos données.

3. **Capturer des données d'un oscilloscope réel**
   Connectez l'instrument et configurez la communication depuis le panneau du nœud Oscilloscope.
   Le nœud acquiert le signal et le délivre par ses ports de sortie (un par canal).
   Connectez ces ports à d'autres nœuds de traitement ou au visualiseur.

4. **Exporter un graphique pour un rapport**
   Connectez le signal désiré à un nœud Exportateur de graphiques.
   Sélectionnez le nœud (clic droit) pour configurer l'aspect visuel du graphique.
   Des profils peuvent également être chargés/enregistrés pour accélérer l'obtention de graphiques prêts pour les rapports en obtenant le fichier image dans l'extension choisie.

---

## À quoi sert tout cela

Cette architecture est conçue pour que vous puissiez vous concentrer sur l'analyse de signaux sans vous soucier de la façon dont le programme est organisé en interne. Chaque composant a une fonction claire et travaille ensemble pour offrir une expérience fluide, de la simulation à l'instrumentation réelle, en passant par la personnalisation visuelle et l'exportation des résultats.

Si jamais vous avez besoin d'étendre les capacités de FloWorks (par exemple, en ajoutant de nouveaux types de nœuds ou en connectant un instrument différent), sachez qu'il existe une structure modulaire qui le permet, bien que cela relève des développeurs. En tant qu'utilisateur final, profitez de la flexibilité que ce design vous offre.
