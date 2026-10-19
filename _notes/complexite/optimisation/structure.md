---
layout: default
parent: "Optimisation"
title: "Structure de données"
nav_order: 1
published: true
---

# Structure de données

## Objectif
Choisir ou adapter les structures de données pour faciliter le traitement et améliorer les performances.

## Explication
Une structure de données bien choisie peut transformer un algorithme inefficace en une solution performante. Elle permet d'accéder plus rapidement aux données, de simplifier les opérations, et parfois d'éviter des calculs inutiles.

## Pourquoi c’est utile
- Réduction du temps d’accès aux données
- Simplification du code
- Préparation à des algorithmes plus efficaces

## Exemple

### Avant
```java
// Recherche d'un élément dans un tableau
int[] data = {9, 5, 3, 7};
int target = 7;
boolean found = false;
for (int i = 0; i < data.length; i++) {
    if (data[i] == target) {
        found = true;
        break;
    }
}
```

### Après
```java
// Utilisation d'un tableau trié et recherche binaire
int[] data = {3, 5, 7, 9};
int target = 7;
int left = 0;
int right = data.length - 1;
boolean found = false;
while (left <= right) {
    int mid = (left + right) / 2;
    if (data[mid] == target) {
        found = true;
        break;
    } else if (data[mid] < 7) {
        left = mid + 1;
    } else {
        right = mid - 1;
    }
}
```

## Autres exemples
- Utiliser un **arbre binaire de recherche** au lieu d’un tableau pour des insertions et recherches fréquentes.
- Utiliser une **table de hachage** au lieu d’un arbre pour des accès directs en temps constant.
- Utiliser une **pile** pour simuler des appels récursifs dans une version itérative.
