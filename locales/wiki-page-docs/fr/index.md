---
title: FloWorks
description: Laboratoire visuel universel pour le traitement du signal, l'instrumentation scientifique et l'automatisation.
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## Laboratoire visuel universel pour les signaux, l'instrumentation et l'IA

Traitement scientifique • DSP • VISA/SCPI • Automatisation • Apprentissage automatique

![Capture d'écran de FloWorks](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[Premiers pas avec FloWorks](getting-started.md){ .md-button }
[Anatomie de l'Interface Principale](interface-anatomy.md){ .md-button .md-button--primary }
[Philosophie](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## Qu'est-ce que FloWorks ?
FloWorks est un **laboratoire visuel open source** (Python + PySide6) où vous construisez des systèmes en connectant des blocs (nœuds) au lieu d'écrire des lignes de code.

Imaginez une toile numérique où vous reliez des générateurs de signaux, des filtres mathématiques, des contrôleurs de matériel (VISA/SCPI) et des modèles d'Intelligence Artificielle par des câbles virtuels. Tout repose sur le **flux de données** : vous connectez la sortie d'un bloc à l'entrée d'un autre pour traiter des informations, automatiser des équipements ou analyser des résultats en temps réel.

Il s'adresse aux étudiants, chercheurs, ingénieurs et à toute personne souhaitant expérimenter, apprendre ou prototyper des systèmes complexes de manière intuitive, sans la barrière de la programmation traditionnelle.

### Mission
Centraliser le flux de travail expérimental dans un seul outil visuel, ouvert et accessible. Nous voulons que les utilisateurs se concentrent sur *expérimenter et découvrir*, et non sur la lutte contre la complexité logicielle ou les coûts des licences.

### Vision
Un monde où la seule barrière entre une idée expérimentale et son exécution est la curiosité de l'expérimentateur. FloWorks aspire à être la plateforme de référence pour la science et l'ingénierie, construite par et pour la communauté mondiale, en éliminant les murs des outils propriétaires.

### Principes
* **Liberté Totale :** Les connaissances et les outils doivent être accessibles à tous. FloWorks est gratuit et s'engage en faveur d'un cœur ouvert et extensible.
* **Extensibilité Infinie :** S'il manque un bloc, n'importe qui peut le créer et l'intégrer à l'écosystème en utilisant Python.
* **Transparence Visuelle :** Chaque étape du processus peut être inspectée, déboguée et comprise graphiquement.
* **Connexion avec le Monde Réel :** Ce n'est pas seulement de la simulation ; il permet de contrôler directement depuis la toile une instrumentation scientifique réelle.

Contrairement aux outils fermés ou hautement spécialisés, FloWorks est conçu comme un écosystème modulaire extensible où chaque composant est un nœud réutilisable et connectable.

---

## Capacités principales

<div class="grid cards" markdown>

-   **:material-puzzle-outline: Écosystème de Nœuds Extensible**

    Catalogue technique organisé en couches : Sources, Traitement, Contrôle, Matériel et Scripting.

    Registre dynamique, sérialisation déclarative et contrats clairs pour un développement rapide.

    [:material-arrow-right: Référence des Nœuds](node-reference.md)

-   **:material-connection: Intégration VISA/SCPI**

    Connexion directe avec des oscilloscopes, des mètres LCR et des générateurs.

    Support multicanal, simulation intégrée via `PyVISA-py` et gestion du pare-feu en mode portable.

    [:material-arrow-right: Instrumentation](instrumentation.md)

-   **:material-package-variant-closed: Format `.sflow` Portable**

    Standard ZIP autonome avec graphe JSON, tableaux `.npy` et métadonnées.

    Reproductibilité totale des expériences et normalisation DPI automatique.

    [:material-arrow-right: Format .sflow](sflow-format.md)

-   **:material-translate: Internationalisation Avancée**

    Changement de langue à chaud sans redémarrer l'application.

    Traductions JSON hiérarchiques et persistance des préférences.

    [:material-arrow-right: Guide i18n](translation-guide.md)

-   **:material-tools: SDK et Développement Rapide**

    Modèle de base (`template_node.py`), mixin de sérialisation et guides pas à pas.

    Architecture prête pour les plugins et l'expansion communautaire.

    [:material-arrow-right: Créer des Nœuds](adding-a-new-node.md)

</div>

---

## Domaines d'application

| Domaine | Applications |
|---------|--------------|
| 🎓 **Éducation** | Physique, électronique, mathématiques, laboratoires STEM |
| ⚙️ **Ingénierie** | DSP, contrôle, instrumentation, métrologie |
| 🤖 **IA** | ML, optimisation, pipelines hybrides |
| 🔬 **Recherche** | Automatisation et acquisition de données |
| 🔌 **Matériel** | VISA/SCPI, simulation et systèmes hybrides |

---

!!! tip "Nouveau sur FloWorks ?"

    Commencez par la section **Premiers pas avec FloWorks**, puis **Anatomie de l'Interface** pour comprendre l'architecture de l'interface graphique et explorez enfin **Architecture Générale** pour comprendre le flux de données et la structure du moteur topologique.

---

<div markdown="1" style="text-align: center;">

## FloWorks

Traitement Visuel • Instrumentation • Science • IA

<small>Documentation construite avec MkDocs Material</small>

</div>
