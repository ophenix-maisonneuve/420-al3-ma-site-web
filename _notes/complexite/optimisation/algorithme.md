---
layout: default
parent: "Optimisation"
title: "Algorithme"
nav_order: 2
published: true
---

# Algorithme plus efficace

## Objectif
Remplacer un algorithme par un autre qui résout le même problème avec une meilleure complexité.

## Explication
Certains algorithmes sont plus adaptés à certaines situations. Choisir le bon algorithme peut réduire considérablement le temps d'exécution.

## Pourquoi c’est utile
- Réduction immédiate de la complexité temporelle
- Meilleure évolutivité sur des grands ensembles de données
- Souvent peu coûteux à implémenter

---
## Exemple

### Avant
```java
// Tri par sélection
int[] arr = {5, 2, 9, 1};
for (int i = 0; i < arr.length - 1; i++) {
    int minIndex = i;
    for (int j = i + 1; j < arr.length; j++) {
        if (arr[j] < arr[minIndex]) {
            minIndex = j;
        }
    }
    int temp = arr[i];
    arr[i] = arr[minIndex];
    arr[minIndex] = temp;
}
```

### Après
```java
// Tri rapide (QuickSort)
public static void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

public static int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;
    return i + 1;
}
```

## Autres exemples
- Utiliser le **tri fusion** pour des collections très grandes au lieu du tri à bulles.
- Utiliser la **recherche binaire** sur une collection triée au lieu d’une recherche linéaire.
