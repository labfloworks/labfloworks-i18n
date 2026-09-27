# Notes Adhésives – Guide de l'utilisateur

## Que sont les notes adhésives ?

Les notes adhésives sont de petits blocs de texte que vous pouvez placer librement sur le diagramme. Elles servent à :

- Ajouter des rappels, des titres ou des explications directement sur le canevas.
- Créer des tutoriels pas à pas qui guident quiconque utilise votre projet.
- Documenter des parties du flux de travail sans quitter FloWorks.
- Laisser des commentaires pour vous-même ou d'autres collaborateurs.

Les notes sont redimensionnables (en faisant glisser leurs coins), peuvent être déplacées n'importe où sur le diagramme et sont enregistrées avec le projet. Lorsque vous ouvrez un fichier `.sflow`, toutes les notes apparaissent exactement là où vous les avez laissées.

---

## La nouveauté : notes multilingues

Les notes adhésives peuvent afficher automatiquement le texte dans la langue que vous choisissez pour l'application.
Au lieu d'écrire le message final dans une seule langue, vous pouvez insérer des **marqueurs spéciaux** qui se traduiront eux-mêmes lorsque vous changerez la langue de FloWorks.

Ainsi, une seule note peut être lue en espagnol, en anglais ou dans toute autre langue disponible sans avoir besoin d'éditer le texte à chaque fois.

---

## Comment écrire une note multilingue

À l'intérieur d'une note (créez-en une par double-clic ou avec le bouton 📝 de la barre d'outils), vous pouvez utiliser deux types de marqueurs :

### 1. Avec le mot `tr(…)`
Tapez `tr("clé")` et remplacez `clé` par un nom descriptif de la phrase.

Exemple :

tr("tutorial.etape1.titre")
tr("tutorial.etape1.message")

### 2. Avec des doubles accolades `{{…}}`
Tapez `{{clé}}` de la même manière.

Exemple :

{{tutorial.etape1.titre}}
{{tutorial.etape1.message}}


Les deux formats fonctionnent de la même manière ; choisissez celui qui vous convient le mieux (vous pouvez même les combiner dans la même note).

> **Important** : Le texte que vous voyez lorsque vous modifiez la note contient les marqueurs originaux (par exemple `{{tutorial.etape1.titre}}`).
> Lorsque vous avez terminé l'édition et que vous revenez à la vue normale du diagramme, les marqueurs sont remplacés par la phrase traduite dans la langue actuelle de l'application.

---

## Comportement lors du changement de langue

- Si vous changez la langue depuis le menu de FloWorks (par exemple, de l'espagnol vers l'anglais), **toutes les notes adhésives contenant des marqueurs se mettent à jour automatiquement**.
- Il n'est pas nécessaire de fermer et rouvrir le projet, ni de toucher chaque note manuellement.
- Les notes qui ne contiennent que du texte simple (sans marqueurs) ne sont pas affectées ; elles affichent la même chose dans n'importe quelle langue.

---

## Avantages de l'utilisation des marqueurs

- **Tutoriels multilingues instantanés** – Une seule note suffit pour guider les utilisateurs de différentes langues.
- **Cohérence** – Si vous modifiez la traduction en un seul endroit (le fichier de langues que votre équipe de développement maintient), toutes les notes qui utilisent cette clé se mettront à jour.
- **Maintenance facile** – Vous pouvez écrire le contenu une seule fois et le réutiliser dans plusieurs notes.
- **Flexibilité** – Combinez du texte fixe avec des marqueurs. Par exemple :

---

## Exemple pratique : un tutoriel pas à pas

Supposons que vous vouliez ajouter une note expliquant la première étape d'un tutoriel.
En mode édition vous écrivez :

🎯 ÉTAPE 1
{{tutorial.etape1.titre}}
{{tutorial.etape1.message}}


Lorsque vous avez terminé l'édition et que vous utilisez l'application en espagnol, vous verrez :

🎯 ÉTAPE 1
¡Bienvenido a FloWorks!
Arrastre un nodo fuente de señal para comenzar.


Si vous passez la langue à l'anglais, la même note affichera :

🎯 ÉTAPE 1
Welcome to FloWorks!
Drag a signal source node to begin.


Et ainsi de suite pour toute autre langue que vous avez configurée.

---

## Résumé

- Les notes adhésives enrichissent vos diagrammes avec des informations textuelles.
- Elles peuvent maintenant être **multilingues** en utilisant les marqueurs `tr("clé")` ou `{{clé}}`.
- En édition vous verrez les clés ; en visualisation, le texte traduit.
- Changez la langue de l'application et toutes les notes s'adapteront instantanément.
- Parfait pour créer de la documentation visuelle, des tutoriels ou des avis qui doivent fonctionner en plusieurs langues.

Profitez de cette fonctionnalité pour rendre vos projets plus accessibles et plus faciles à partager !
