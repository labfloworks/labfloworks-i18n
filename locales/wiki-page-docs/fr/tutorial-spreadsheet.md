# 📊 Tutoriel de Spreadsheet

Feuille de calcul légère de style Excel, intégrable.

---

## 1. Qu'est-ce que Spreadsheet ?

C'est un composant de feuille de calcul qui offre :

- Des cellules avec support des formules (commencent par `=`)
- Des fonctions prédéfinies (SUM, AVERAGE, CONDITIONNELLES, etc.)
- Des opérateurs arithmétiques, logiques et de comparaison
- Une interface minimaliste, idéale pour l'intégration dans des applications Qt

---

## 2. Navigation de base

- Cliquez sur une cellule pour la sélectionner.
- Tapez directement pour saisir du texte ou des nombres.
- Pour écrire une **formule**, commencez par `=` (ex. `=SUM(A1:A5)`).
- Appuyez sur **Entrée** pour confirmer l'édition.
- Utilisez les touches fléchées ou la souris pour vous déplacer.

---

## 3. Formules : fonctions disponibles

Toutes les fonctions s'écrivent en majuscules et acceptent des plages (ex. `A1:A5`) ou des arguments séparés par des virgules.

| Fonction                 | Ce qu'elle fait                  | Exemple                        |
| ------------------------ | -------------------------------- | ------------------------------ |
| `SUM`                    | Somme de nombres                 | `=SUM(A1:A5)`                  |
| `AVG` / `AVERAGE`        | Moyenne                          | `=AVG(A1:A5)`                  |
| `COUNT`                  | Compte les nombres (non vides)   | `=COUNT(A1:A5)`                |
| `MAX`                    | Valeur maximale                  | `=MAX(A1:A5)`                  |
| `MIN`                    | Valeur minimale                  | `=MIN(A1:A5)`                  |
| `ABS`                    | Valeur absolue                   | `=ABS(A1)`                     |
| `ROUND`                  | Arrondi (2e argument = décimales) | `=ROUND(A1, 2)`               |
| `IF`                     | Conditionnel (si, alors, sinon)  | `=IF(A1>10, "Oui", "Non")`     |
| `CONCAT` / `CONCATENATE` | Concatène du texte               | `=CONCAT(A1, " ", B1)`         |
| `LEN`                    | Longueur du texte                | `=LEN(A1)`                     |
| `INT`                    | Partie entière                   | `=INT(A1)`                     |
| `SQRT`                   | Racine carrée                    | `=SQRT(A1)`                    |

### 3.1. Notes sur les fonctions

- Les plages s'indiquent avec **deux points** : `A1:A5` inclut toutes les cellules de A1 à A5.
- Les fonctions peuvent être imbriquées : `=SUM(A1:A5) + MAX(B1:B5)`.
- Les arguments texte doivent être entre guillemets doubles ou simples.

---

## 4. Opérateurs sans fonctions

En plus des fonctions, vous pouvez utiliser des opérateurs directement dans la formule. La syntaxe est similaire à Python.

### 4.1. Arithmétiques

| Opération      | Exemple                   |
| -------------- | ------------------------- |
| Addition       | `=A1+A2+A3`               |
| Soustraction   | `=A1-A2`                  |
| Multiplication | `=A1*B1`                  |
| Division       | `=A1/B1`                  |
| Modulo         | `=A1%B1`                  |
| Puissance      | `=A1**2`  (ou `=A1^2`)   |

### 4.2. Comparaisons

Elles retournent `True` ou `False` (affichés comme `Vrai` / `Faux`).

| Opérateur | Signification     | Exemple            |
| --------- | ----------------- | ------------------ |
| `>`       | Supérieur à       | `=A1>B1`           |
| `<`       | Inférieur à       | `=A1<B1`           |
| `>=`      | Supérieur ou égal | `=A1>=10`          |
| `<=`      | Inférieur ou égal | `=A1<=10`          |
| `==`      | Égal              | `=A1==B1`          |
| `!=`      | Différent         | `=A1!=B1`          |

### 4.3. Logiques et conditionnels

Vous pouvez combiner des conditions avec `and`, `or`, `not`.

```excel
= A1>5 and B1<10
= not(A1==0)
= 10 if A1>5 else 0
```

L'opérateur ternaire `if` `else` est également supporté directement.

---

## 5. Exemples pratiques

### 5.1. Somme des ventes

Supposons que vous avez des ventes dans `B2:B10` et que vous voulez le total :

```excel
=SUM(B2:B10)
```

### 5.2. Remise conditionnelle

Si le total (dans `B12`) dépasse 100, appliquez une remise de 10 % ; sinon, 0 :

```excel
= IF(B12>100, B12*0.9, B12)
```

### 5.3. Moyenne et comptage

Moyenne des notes dans `C2:C20`, mais seulement s'il y a au moins 5 valeurs :

```excel
= IF(COUNT(C2:C20)>=5, AVG(C2:C20), "Données insuffisantes")
```

### 5.4. Texte combiné

Joindre le prénom (A2) et le nom (B2) avec un espace :

```excel
= CONCAT(A2, " ", B2)
```

### 5.5. Racine carrée d'un nombre

```excel
= SQRT(A1)
```

### 5.6. Arrondi à 2 décimales

```excel
= ROUND(A1, 2)
```

---

## 6. Conseils et astuces

- **Références relatives/absolues :** pour l'instant, toutes les références sont relatives (comme Excel). `$A$1` n'est pas encore supporté.
- **Plages dynamiques :** vous pouvez utiliser des plages comme `A:A` (toute la colonne) ou `1:1` (toute la ligne).
- **Autocomplétion :** en tapant `=`, un menu avec les fonctions disponibles apparaît.
- **Erreurs :** si une formule est invalide, la cellule affichera `#ERROR` et le message détaillé dans la barre d'état.
- **Recalcul :** les formules se mettent à jour automatiquement lors de la modification des cellules dépendantes.

---

## 8. Questions fréquemment posées

**Comment exporter les données ?**
Pour l'instant, il n'y a pas d'exportation native, mais vous pouvez accéder aux données via le modèle interne.

**Supporte-t-il les graphiques ?**
Non, c'est une feuille de calcul basique. Vous pouvez la combiner avec d'autres widgets pour la visualisation.

---

Profitez de l'utilisation de la feuille de calcul légère !

© 2026 — FloWorks
