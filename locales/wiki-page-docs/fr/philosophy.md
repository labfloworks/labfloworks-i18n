# Philosophie

## Vue d'ensemble

FloWorks est une application de bureau qui vous permet de créer des chaînes de traitement de signaux par des diagrammes de flux visuels.
Glissez-déposez, connectez et configurez des nœuds ; le résultat est calculé et affiché en temps réel.
Travaillez avec des signaux simulés ou connectez de vrais instruments (oscilloscopes, générateurs, multimètres LCR) sans avoir besoin d'écrire de code, bien qu'un environnement de script puissant soit disponible si vous souhaitez étendre les fonctionnalités.

---

## Caractéristiques principales

- **Diagrammes interactifs** – Construisez votre flux de travail en reliant des nœuds avec des lignes qui représentent le flux de données.
- **Traitement en temps réel** – Chaque modification se reflète immédiatement dans les graphiques et visualisations.
- **Simulation et matériel réel** – Générez des signaux de test ou capturez des données directement depuis des instruments de laboratoire.
- **Nœud de script avancé** – Intégrez votre propre code Python avec aide d'autocomplétion, paramètres dynamiques éditables et mémoire persistante entre les exécutions.
- **Visualisation professionnelle** – Signaux, spectres, spectrogrammes et graphiques de haute qualité prêts à exporter.
- **Multilingue** – L'interface détecte la langue du système et permet de changer entre espagnol, anglais et autres langues à tout moment.
- **Thèmes visuels** – Mode sombre, clair et contraste élevé pour s'adapter à vos préférences ou besoins d'accessibilité.
- **Gestion complte de projets** – Enregistrez votre travail dans des fichiers `.sflow` et récupérez-le exactement comme vous l'avez laissé, avec annulation et refaire illimités.

---

## Comment travailler avec FloWorks

### Nœuds
Un nœud est une pièce du traitement. Ils sont organisés en trois catégories :

- **Sources** – Insèrent des signaux au début du flux. Par exemple, un oscilloscope (réel ou simulé), un générateur de fonctions ou une opération mathématique.
- **Traitement** – Transforment les données. Additions, soustractions, conditionnels, filtres… même un nœud spécial pour écrire vos propres scripts Python.
- **Puits** – Affichent ou exportent les résultats. Le visualiseur graphique et l'exportateur de graphiques professionnel sont les plus utilisés.

### Connexions
Les unions entre nœuds sont dessinées comme des courbes douces ou des lignes orthogonales. Une animation de flux indique en tout temps la direction des données. Le système organise automatiquement les câbles pour qu'ils ne se chevauchent pas.

### Visualisation
Chaque fois qu'un nœud produit un signal, celui-ci peut être vu dans le panneau de graphiques intégré. Vous pouvez explorer différentes représentations (forme d'onde, spectre, spectrogramme) et ajuster l'échelle avec la souris.

---

## Nœuds mis en évidence
Ce sont les nœuds minimaux indispensables pour que la philosophie du programme ait du sens.

### Nœud générateur de signaux
Source de signaux qui peut générer des simulations personnalisées de formes d'onde au goût de l'utilisateur. Il permet de sélectionner ou de saisir la forme d'onde requise depuis un menu contextuel.

### Nœud de script
Un environnement de programmation complet dans le diagramme :

- **Éditeur avec coloration syntaxique**, autocomplétion et console d'erreurs.
- **Paramètres dynamiques** – Définissez des variables éditables depuis le panneau du nœud sans modifier le code.
- **Ports configurables** – Ajoutez des entrées et sorties supplémentaires directement depuis l'éditeur.
- **État persistant** – Enregistrez des valeurs entre les exécutions ; tout est stocké avec le projet.

### Exportateur de graphiques
Nœud puits qui génère des images de haute qualité pour des rapports ou des publications. Permet de configurer la taille, la résolution, le format, entre autres.

---

## Personnalisation

- **Langue** – L'application détecte automatiquement la langue du système et enregistre votre préférence. Vous pouvez la changer depuis le menu sans redémarrer.
- **Apparence** – Choisissez entre le thème sombre, clair ou à contraste élevé selon la lumière ambiante ou vos besoins visuels.

---

## Projets et fichiers

Enregistrez votre diagramme complet dans un fichier `.sflow`.
En l'ouvrant, vous récupérerez tous les nœuds, connexions, scripts, paramètres et configurations de visualisation.
Les actions d'annulation et de refaire vous permettent d'expérimenter sans crainte de perdre le travail précédent.

---

FloWorks est conçu pour que vous puissiez vous concentrer sur l'analyse de signaux et non sur les détails techniques de l'implémentation. Glissez-déposez, connectez et découvrez.
