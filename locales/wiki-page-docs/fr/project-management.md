---
title: Gestion de projets
description: Comment enregistrer, ouvrir, exporter et protéger vos flux dans FloWorks.
---

# 📁 Gestion de projets

FloWorks enregistre vos flux dans des fichiers avec l'extension **`.sflow`**. Ces fichiers contiennent toutes les informations du projet : nœuds, connexions, configuration et notes adhésives.

---

## Créer, ouvrir et enregistrer

| Action | Menu | Raccourci |
|--------|------|-----------|
| **Nouveau projet** | Fichier → Nouveau | `Ctrl + N` |
| **Ouvrir un projet** | Fichier → Ouvrir | `Ctrl + O` |
| **Enregistrer** | Fichier → Enregistrer | `Ctrl + S` |
| **Enregistrer sous…** | Fichier → Enregistrer sous… | `Ctrl + Maj + S` |

**Règle d'or :**
Les flux sont **entièrement compatibles entre toutes les versions** de FloWorks (Core, Lite, Pro). Vous n'avez pas besoin de convertir ni de modifier quoi que ce soit : il suffit d'ouvrir et d'exécuter.

---

## Exportation et importation

- Pour **partager un flux**, copiez le fichier `.sflow` sur un autre ordinateur.
- Pour **importer un flux externe**, utilisez **Fichier → Ouvrir** et sélectionnez le fichier.
- Si vous devez **exporter des données numériques** (par exemple, vers CSV), utilisez l'outil **Feuille de calcul** du panneau latéral et enregistrez le tableau depuis là.

---

## Récupération après fermetures inattendues

FloWorks **n'enregistre pas automatiquement**. C'est pourquoi il est important de :

- Enregistrer fréquemment (`Ctrl + S`), surtout avant d'exécuter des flux avec du matériel réel.
- Si l'application se ferme de façon inattendue, les modifications non enregistrées pourraient être perdues.
- Pour travailler en toute tranquillité, habituez-vous à enregistrer après chaque modification importante.

---

## Organisation recommandée

- Créez un dossier par projet ou client, et y enregistrez tous les fichiers `.sflow` associés.
- Utilisez des **notes adhésives** à l'intérieur du canevas pour documenter des sections du flux.
- Attribuez des **noms descriptifs aux nœuds** (double-clic → nom) pour qu'il soit plus facile de trouver et comprendre le flux des semaines plus tard.

---

## Bonnes pratiques

- Avant d'exécuter un flux avec des instruments réels, enregistrez le fichier.
- Si vous travaillez en équipe, utilisez un système de contrôle de versions (Git, copies manuelles) pour ne pas écraser des flux importants.
- Faites des sauvegardes de flux de calibration ou de diagnostic critiques.

---

> **Conseil :** Un flux bien organisé et enregistré est la base d'un travail professionnel dans FloWorks. Ne sous-estimez pas le pouvoir d'un nom clair et d'un dossier ordonné.
