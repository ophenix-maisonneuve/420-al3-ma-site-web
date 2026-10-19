---
layout: default
parent: "Notation Grand O"
title: "Complexité O(2<sup>n</sup>) - Exponentielle"
nav_order: 5
published: true
---

# Complexité O(2<sup>n</sup>) - Exponentielle

Un algorithme en O(2<sup>n</sup>) devient rapidement impraticable. Chaque ajout d’élément double le nombre d’opérations. On retrouve cette complexité dans des algorithmes récursifs qui explorent toutes les combinaisons possibles, comme la génération de sous-ensembles.

## Exemple
```java
public class ExempleO2n {
    public static void generateSubsets(int[] array, int index, int[] subset) {
        if (index == array.length) {
            // Affiche le sous-ensemble
            for (int i = 0; i < subset.length; i++) {
                System.out.print(subset[i] + " ");
            }
            return;
        }
        subset[index] = 0;
        generateSubsets(array, index + 1, subset);
        subset[index] = array[index];
        generateSubsets(array, index + 1, subset);
    }
}
```

## Allure de la courbe

![Complexité O(2<sup>n</sup>)](../assets/images/complexite-o2n.png)
