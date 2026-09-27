---
title: Référence Technique des Nœuds
description: Catalogue actualisé, contrats d'extension et capacités avancées du système de nœuds de FloWorks
---

# 🧩 Référence Technique des Nœuds

FloWorks ne dépend pas d'un catalogue statique. Il utilise un **système d'enregistrement dynamique** basé sur des contrats clairs. Cela permet d'étendre la plateforme sans toucher au moteur topologique. Ci-dessous se trouvent le catalogue implémenté, les capacités techniques réelles et le protocole pour l'étendre de manière sûre.

---

## 📂 Catégories du Noyau

=== "📦 Vue par Couches"
    <div class="grid cards" markdown>

    - **📥 Sources/Entrée**
      Génèrent ou capturent les signaux initiaux. Supportent la simulation intégrée, le matériel réel (VISA/SCPI) et le mode multicanal.
    - **⚙️ Traitement**
      Transforment, combinent ou analysent les données. Préservent la dimensionalité et interpolent automatiquement lorsque nécessaire.
    - **🔀 Contrôle/Flux**
      Bifurquent, itèrent ou conditionnent l'exécution. Incluent le support natif pour les signaux d'activation.
    - **🐍 Scripting/Avancé**
      Exécutent du code Python dynamique avec ports paramétriques (`# @param`), ports dynamiques (`# @input`/`# @output`) et persistance d'état (`persist`).
    - **🔌 Matériel/Instrumentation**
      Interfaces pour oscilloscopes, multimètres LCR et générateurs.
    - **📤 Sortie/Exportation**
      Visualisent, exportent ou archivent les résultats. Supportent les thèmes visuels, les profils utilisateur et le format professionnel (PNG/PDF/SVG).

    </div>

---

## 📋 Catalogue Technique Implémenté

| Nœud | Type | Responsabilité Principale | Caractéristiques Clés |
|------|------|---------------------------|------------------------|
| `SumNode` | Traitement | Opérateur arithmétique (+, -, *, /) pour deux entrées. | Interpole automatiquement les signaux de résolution différente (FFTs). Préserve la dimensionalité. |
| `RhombusNode` | Contrôle | Conditionnel (bifurcation Oui/Non). | Deux ports de sortie. Évalue la condition par seuil ou logique booléenne. |
| `TriggerNode` | Contrôle | Itérateur/accumulateur avec activation externe. | Reçoit `(x, y, "trigger")`. Accumule jusqu'à N itérations et émet le résultat empilé/moyenné. |
| `ScriptNode` | Avancé | Environnement de scripting Python intégré. | QScintilla, autocomplétion, `# @param`, ports dynamiques, `persist`, modèles, console d'erreurs, interpréteur externe avec timeout. |
| `OscilloscopeNode` | Matériel | Capture depuis oscilloscopes (SDS) ou multimètres LCR. | Mode simulation, dialogue de pare-feu intégré, **support multicanal** (`out_primary`, `out_secondary`), menu "Afficher canal". |
| `GeneratorNode` | Source | Envoie des signaux aux générateurs (SDG) ou simule des sorties. | Configuration de modulation/balayage, dialogue de simulation intégré. |
| `GraphExporterNode` | Sortie | Exportateur de graphiques professionnel. | Configuration par double-clic, axes personnalisés, thèmes, profils enregistrés, etc. |

---

## 🔍 ScriptNode : Capacités Essentielles

> **🐍 Environnement de Scripting Intégré**
>
> - **Éditeur de code intégré :** Coloration syntaxique basique, numérotation des lignes et pliage de code.
> - **Panneau de paramètres dynamiques :** Les directives `# @param NOM : type = valeur` injectent des contrôles éditables (spinbox, champ de texte, etc.) dans le panneau latéral.
> - **Ports dynamiques :** `# @input nom` et `# @output nom` créent des ports en temps réel. Le script reçoit un dictionnaire `inputs` et retourne `outputs`.
> - **Persistance d'état :** Dictionnaire global `persist` qui maintient les valeurs entre les exécutions.
> - **Modèles et Import/Export :** Menu déroulant avec scripts de base. L'utilisateur peut enregistrer ses scripts dans `nodes/script_node/templates/` ou importer/exporter des fichiers `.py` externes.
> - **Console d'erreurs intégrée :** Affiche les échecs de syntaxe/exécution avec la ligne exacte signalée dans l'éditeur.
> - **Aide et i18n :** Info-bulles contextuelles, bouton `?` avec guide rapide, et tous les textes utilisent `tr()` pour la traduction.
> - **Interpréteur externe avec timeout :** Chemin configurable (`# @python_path` ou bouton "Parcourir…"). Exécution isolée avec limite de temps et fallback vers l'interpréteur interne.
> - **Sérialisation complète :** Enregistre le script, les paramètres, les ports dynamiques et l'état `persist`. Lors du chargement d'un `.sflow`, il reconstruit automatiquement les ports et paramètres.

---

## 📚 Ressources Liées

- [📖 Carte du Code et Architecture](architecture-ii.md) → Responsabilités par module et flux de travail.
- [🌐 Guide d'Internationalisation (i18n)](i18n.md) → Comment ajouter des langues et gérer les clés `tr()`.
- [🛠️ Ajouter un Nouveau Nœud (Tutoriel)](adding-a-new-node.md) → Pas à pas avec exemples pratiques.
- [📦 Guide de Build et Distribution](build.md) → Empaquetage PyInstaller, hooks et signatures numériques.

---

💡 **Il manque un nœud dans ce catalogue ?**
FloWorks est conçu pour être extensible. Si vous avez besoin d'un nœud qui n'existe pas, créez-le en suivant le contrat de `BaseNode` et enregistrez-le. La communauté et le futur marketplace élargiront continuellement l'écosystème sans casser la compatibilité.
