---
layout: default
title: "Optimisation d'algorithmes"
nav_order: 5
has_toc: false
published: true
---

# Exercices : Optimisation d'algorithmes

## Exercice 1

### Objectif
- Comprendre l’impact du choix de structure sur les performances.
- Appliquer un algorithme plus efficace une fois la structure adaptée.
- Comparer les performances entre deux approches.

### Contexte

Vous avez un tableau d’entiers non trié représentant des identifiants d’étudiants :

```java
int[] ids = { 42, 17, 8, 23, 4, 16, 15, 9, 30, 2 };
```

Un logiciel utilise ce tableau pour faire des recherches sur les identifiants. **Cette opération de recherche est effectuée très fréquemment**, il est donc crucial de l’optimiser.

### Algorithme de recherche actuel

```java
public static boolean contains(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == target) {
             return true;
        }
    }
    return false;
}
```

**Question**
> Quelle est la complexité grand O de l'algorithme ci-dessus?

### Optimisation de la structure de données

Proposez une amélioration possible sur la **structure de données** (le tableau)

**Question**
> Comment cette amélioration permet (ou permettra) une meilleure optimisation de la recherche?

{: .highlight}
> Il est possible que le changement de structure lui-même n'apporte pas d'amélioration, mais permette l'utilisation d'un algorithme différent dans la section suivante...

### Optimisation de l'algorithme de recherche
Proposez une amélioration possible sur **l'algorithme de recherche**.

**Question**
> Quelle est la nouvelle complexité de l'algorithme de recherche après votre modification?

{: .highlight}
> Votre solution dépendra très probablement du changement que vous aurez apporté à la structure de données de la question précédente.


### Discussion en groupe
- Quelle version est plus rapide?
- Quel est le coût du tri initial?
- Dans quels cas vaut-il la peine de trier le tableau avant de chercher?
- Peut-on réutiliser le tri pour plusieurs recherches?


## Exercice 2

### Objectif
- Identifier les limites d’une approche récursive naïve.
- Réduire le problème en sous-problèmes.
- Appliquer la programmation dynamique via mémoïsation ou tabulation.
- Comparer les performances et discuter des impacts sur le temps et la mémoire.

### Contexte
Vous devez calculer le n-ième terme de la suite de Fibonacci. Cette opération est effectuée **très fréquemment** dans le programme, et doit donc être **optimisée**.

### Algorithme actuel
```java
public static int fib(int n) {
    if (n <= 1) {
        return n;
    }
    return fib(n - 1) + fib(n - 2);
}
```

- Complexité : O(2<sup>n</sup>)
- Problème : recalculs redondants, pile d’appels récursifs

### Réduction du problème
Identifier que `fib(n)` dépend uniquement de `fib(n-1)` et `fib(n-2)`, donc sous-problèmes réutilisables.

### Programmation dynamique
Implémenter une version optimisée avec mémoïsation ou tabulation.

## Discussion en groupe
- Quelle version est plus rapide?
- Quel est l’impact sur la mémoire?
- Quelle version est plus lisible?