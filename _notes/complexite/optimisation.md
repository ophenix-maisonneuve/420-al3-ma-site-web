---
layout: default
title: "Optimisation"
parent: "Complexité des algorithmes"
nav_order: 2
published: true
---

# Optimisation algorithmique

## Objectif

L’optimisation algorithmique vise à améliorer les performances d’un logiciel en réduisant sa complexité, que ce soit en temps d’exécution ou en utilisation mémoire. Pour guider cette démarche, nous étudierons une méthode structurée qui peut être utilisée pour la grande majorité des problèmes d'optimisation.

## Méthode 

La méthode proposée comporte 5 étapes:
1. Caractérisation du problème
1. Évaluation de la structure de données utilisée
1. Évaluation de l'algorithme utilisé
1. Réduction du problème en sous-problèmes
1. Utilisation de programmation dynamique

---

## 1. Caractérisation du problème

Cette étape est sans doute la plus cruciale — et pourtant, souvent négligée. Avant de se lancer dans une tâche d’optimisation, il faut se poser une question simple mais essentielle :  

> **Avons-nous réellement un problème à résoudre?**

Il peut être tentant de vouloir optimiser dès qu’on voit un algorithme de complexité polynomial (O(n<sup>x</sup>)) ou exponentiel (O(2<sup>n</sup>)). Mais si cet algorithme ne traite qu’un petit volume de données, il se peut qu’il soit **parfaitement adapté** à la situation.

{: .warning}
> Bien que l’optimisation puisse être bénéfique, elle peut aussi avoir des effets négatifs :
>
> - Réduction de la lisibilité du code
> - Maintenabilité compromise
> - Ré-exécution de tests d’intégration
> - Complexification des tests unitaires

### Identifier la nature du problème

Si un **véritable problème** est identifié, il faut ensuite en déterminer la nature :

- Est-il principalement lié au **temps d’exécution**?
- Est-il principalement lié à **l’utilisation de la mémoire**?
- Est-il lié aux **deux** 😨?

{: .astuce}
> Observer les opérations les plus fréquentes (lecture, écriture, recherche, suppression) peut révéler des inefficacités liées à la structure de données ou à l’algorithme utilisé.

### Mesurer la complexité de l'algorithme

La **complexité algorithmique** est une **mesure théorique** du coût d’un algorithme selon la taille de l’entrée. Elle se traduit **concrètement** par :
- du **temps d’exécution** (complexité temporelle),
- ou de la **consommation mémoire** (complexité spatiale).

On la mesure généralement à l'aide de la [notation Grand O](../notes/notation-grand-o).

{: .highlight}
> Autrement dit, **la complexité est une mesure**, tandis que **le temps et la mémoire sont les symptômes observables**. 

---

## 2. Structure de données

### Adapter la structure aux opérations dominantes
Avant de choisir une structure, il est essentiel d’analyser les opérations les plus fréquentes :
- **Lectures fréquentes** : privilégier les structures avec accès rapide (ex. tableau, dictionnaire).
- **Écritures fréquentes** : choisir des structures avec insertion efficace (ex. liste chaînée, arbre équilibré).
- **Recherches fréquentes** : utiliser des structures avec recherche optimisée (ex. arbre binaire de recherche, table de hachage).
- **Suppressions fréquentes** : opter pour des structures avec suppression rapide (ex. file de priorité, liste doublement chaînée).

Cette analyse permet de **maximiser les performances là où ça compte le plus**. 

Une structure de données appropriée peut transformer un algorithme inefficace en une solution performante. Elle permet d’**accélérer l’accès aux données**, de **réduire le nombre d’opérations**, et parfois même de **simplifier la logique du programme**.

{: .highlight}
> Par exemple, utiliser un tableau pour un accès indexé rapide, une pile pour gérer des appels imbriqués, ou une matrice pour représenter des relations entre éléments.

---

## 3. Algorithme

Remplacer un algorithme par un autre qui résout le même problème avec une **meilleure complexité** est souvent l’optimisation la plus directe. Cela implique de **comparer plusieurs stratégies** et de **choisir celle qui convient le mieux** au contexte et aux données.

{: .highlight}
> Par exemple, utiliser le tri rapide **O(n log n)** au lieu du tri par insertion **O(n<sup>2</sup>)**, ou une recherche binaire **O(log n)** au lieu d’une recherche linéaire **O(n)**.

---

## 4. Réduction du problème

Reformuler le problème en une série de **sous-problèmes plus simples ou mieux structurés**.  
Si le problème ne semble pas provenir d’une structure de données ou d’un algorithme inadapté, il peut être utile de **le diviser en étapes ou en cas particuliers**.

Cette décomposition permet souvent de :
- **mieux comprendre la logique du problème**,
- **identifier des solutions partielles plus efficaces**,
- **appliquer des optimisations ciblées** à chaque sous-problème.

{: .highlight}
> Par exemple, une fois le problème réduit, il devient plus facile de voir où une structure spécifique (comme un arbre ou une pile) ou un algorithme particulier (comme un tri différent ou une recherche mieux adaptée) pourrait s’appliquer.

---

## 5. Programmation dynamique

La programmation dynamique est une technique qui consiste à **mémoriser les résultats intermédiaires** pour éviter de recalculer plusieurs fois les mêmes sous-problèmes. Elle est particulièrement utile lorsque le problème présente une **structure récursive avec des répétitions**.

{: .highlight}
> Par exemple, dans le calcul de la suite de Fibonacci, on peut utiliser :
> - **Mémoïsation** (*top-down*) : on stocke les résultats au fur et à mesure des appels récursifs.
> - **Tabulation** (*bottom-up*) : on construit une table de résultats à partir des cas de base.

---

## Résumé

| Nom                      | Description                                          | Avantages                                    | Inconvénients                                 | Quand l’appliquer                                       |
| -------------------------- | ------------------------------------------------------ | ---------------------------------------------- | ------------------------------------------------ | ---------------------------------------------------------- |
| Caractérisation du problème     | Identifier les opérations les plus fréquentes et leur complexité         | Choix plus pertinent des structures et algorighmes     | Peut nécessiter une analyse fine du comportement | Toujours, dès l’analyse du problème                  |
| Structure de données    | Choisir une structure adaptée au problème          | Accès rapide, gain de performance immédiat | Peut nécessiter une refonte du code           | Dès le début, selon les données                       |
| Algorithme | Remplacer l’algorithme par un plus performant       | Réduction directe de la complexité         | Nécessite de connaître plusieurs algos       | Dès qu’un algorithme est identifié comme sous-optimal |
| Réduction du problème  | Décomposer en sous-problèmes plus simples          | Clarifie la logique, prépare pour programmation dynamique         | Peut être abstrait ou difficile à modéliser | Quand le problème semble trop complexe                  |
| Programmation dynamique  | Mémoriser les résultats pour éviter les recalculs | Réduction drastique du temps d’exécution  | Peut augmenter l’utilisation mémoire         | Quand des sous-problèmes se répètent                  |