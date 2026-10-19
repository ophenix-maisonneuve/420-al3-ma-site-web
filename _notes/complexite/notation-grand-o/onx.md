---
layout: default
parent: "Notation Grand O"
title: "Complexité O(n<sup>x</sup>) - Polynomiale"
nav_order: 4
published: true
---
# Complexité O(n<sup>x</sup>) - Polynomiale

## Cas général

Chaque boucle imbriquée ajoute un degré la la complexité. Par exemple, 3 boucles imbriquées impliqueront une complexité **O(n<sup>3</sup>)**.

## Cas spécifique: O(n<sup>2</sup>) - Quadratique

Un exemple courant d'algorithme en O(n<sup>2</sup>) est une comparaison de chaque élément d'un ensemble avec tous les autres. Cela se produit souvent avec deux boucles imbriquées. Par exemple, vérifier les doublons dans un tableau en comparant chaque paire. Si la taille des données double, le temps est multiplié par quatre.

### Exemple
```java
public class ExempleOn2 {
    public static boolean hasDuplicates(int[] array) {
        for (int i = 0; i < array.length; i++) {
            for (int j = i + 1; j < array.length; j++) {
                if (array[i] == array[j]) {
                    return true;
                }
            }
        }
        return false;
    }
}
```

### Allure de la courbe

![Complexité O(n<sup>n</sup>))](../assets/images/complexite-on2.png)

