---
title: Guide d'Internationalisation (i18n)
description: Instructions pas à pas pour ajouter et gérer les traductions dans FloWorks
---

# 🌐 Guide d'Internationalisation (i18n)

Ce document explique comment ajouter une nouvelle langue à FloWorks et gérer efficacement les fichiers de traduction.

---

## ➕ Comment Ajouter une Nouvelle Langue

### Étape 1 : Créer le Fichier JSON
Naviguez vers le dossier `locales/`. Copiez `en.json` et renommez-le en utilisant le code [ISO 639-1](https://fr.wikipedia.org/wiki/ISO_639-1) à deux lettres correspondant (par ex. `fr.json` pour le français, `de.json` pour l'allemand).

### Étape 2 : Traduire les Chaînes
Ouvrez le nouveau fichier JSON dans un éditeur de texte.

!!! warning "Ne Modifiez Pas les Clés"
    **Ne changez jamais les clés** (le côté gauche de chaque paire). Traduisez uniquement les valeurs (le côté droit).

**Original (`en.json`) :**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "File",
    "edit": "Edit"
  }
}
```

**Exemple Traduit (`es.json`) :**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "Archivo",
    "edit": "Editar"
  }
}
```
Assurez-vous que la clé racine `"language_name"` contienne le nom natif de la langue (par ex. `"Français"`, `"Deutsch"`, `"Español"`).

### Étape 3 : Valider le JSON
Vérifiez que le fichier est un JSON valide (pas de virgules finales, guillemets corrects, échappements appropriés). Vous pouvez utiliser des validateurs en ligne comme [JSONLint](https://jsonlint.com) ou exécuter :
```bash
python -m json.tool locales/es.json
```

### Étape 4 : Tester la Nouvelle Langue
1. Démarrez FloWorks.
2. Allez dans **INFO → Langue** et sélectionnez la nouvelle langue.
3. Vérifiez que tous les éléments de l'interface se mettent à jour immédiatement (menus, panneaux, boîtes de dialogue, étiquettes de nœuds, etc.).

### Étape 5 : Détection Automatique (Optionnel)
Si la locale système de l'utilisateur correspond au code de la nouvelle langue, FloWorks l'utilisera automatiquement au premier lancement (à condition qu'aucune préférence précédente n'ait été enregistrée dans `QSettings`).

---

## 🌍 Langues Disponibles
- **Anglais** (`en`) – Langue de base / fallback
- **Espagnol** (`es`)

---

## ⚙️ Notes Importantes et Bonnes Pratiques

!!! info "Mécanisme de Fallback"
    La langue de base est l'**anglais**. Si une clé de traduction est manquante dans un fichier de langue, FloWorks utilise automatiquement la chaîne en anglais comme fallback.

!!! warning "Prévention du Débordement de l'UI"
    Gardez les traductions concises pour éviter les ruptures de mise en page. Si un texte traduit est significativement plus long, envisagez d'abréger ou comptez sur le système de thèmes pour gérer la mise à l'échelle dynamique.

!!! tip "Préserver le HTML et les Placeholders"
    - **Balises HTML :** Conservez toutes les balises HTML exactement telles quelles (par ex. `<h3>`, `<b>`, `<pre>`, `<br>`).
    - **Placeholders :** Maintenez la syntaxe `{variable}` là où elle est utilisée (par ex. `"Langue changée en : {name} ({code})"`). Ne les réordonnez ni ne les supprimez.

---

## 🔗 Documentation Liée
- [📖 Carte du Code et Architecture](architecture-ii.md)
- [📦 Guide de Build et Distribution](build.md)
- [🧩 Référence des Nœuds](node-reference.md)
