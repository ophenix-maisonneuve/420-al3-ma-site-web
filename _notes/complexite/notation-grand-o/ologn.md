---
layout: default
parent: "Notation Grand O"
title: "Complexité O(log n) - Logarithmique"
nav_order: 2
published: true
---

# Complexité O(log n) - Logarithmique

Un algorithme en O(log n) réduit le problème à chaque étape, généralement en le divisant par deux. Plus la taille des données augmente, plus la croissance est lente. Un exemple classique est la recherche binaire dans un tableau trié : à chaque comparaison, on élimine la moitié des éléments.

## Exemple
```java
public class ExempleOlogn {
    public static boolean binarySearch(int[] array, int target) {
        int left = 0;
        int right = array.length - 1;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (array[mid] == target) {
                return true;
            } else if (array[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            } 
        }
        return false;
    }
}
```

## Allure de la courbe

![Complexité O(log n)](../assets/images/complexite-ologn.png)