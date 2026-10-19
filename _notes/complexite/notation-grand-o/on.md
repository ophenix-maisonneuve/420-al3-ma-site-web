---
layout: default
parent: "Notation Grand O"
title: "Complexité O(n) - Linéaire"
nav_order: 3
published: true
---

# Complexité O(n) - Linéaire

Un algorithme en O(n) parcourt tous les éléments une fois. Le temps d’exécution augmente proportionnellement à la taille des données. Par exemple, rechercher un élément dans un tableau non trié : si le tableau double de taille, le temps double aussi.

## Exemple
```java
public class ExempleOn {
    public static boolean contains(int[] array, int target) {
        for (int i = 0; i < array.length; i++) {
            if (array[i] == target) {
                return true;
            } 
        }
        return false;
    }
}
```

## Allure de la courbe

![Complexité O(n)](../assets/images/complexite-on.png)
