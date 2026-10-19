---
layout: default
parent: "Optimisation d'algorithmes"
title: "Exercice 1 - Solution"
nav_order: 1
has_toc: false
---

# Solution 1: Recherche par tableau indexé par valeur

## Description
Cette méthode consiste à utiliser un tableau où chaque index représente une valeur possible. Si une valeur est présente, on marque son index dans le tableau.

## Avantages
- Complexité en temps : O(1)
- Très rapide pour des recherches répétées

## Inconvénients
- Consomme beaucoup de mémoire si les valeurs sont dispersées ou très grandes
- Nécessite que les valeurs soient connues et bornées

## Solution
```java
public class RechercheParIndex {
    public static void main(String[] args) {
        int[] ids = { 42, 17, 8, 23, 4, 16, 15, 9, 30, 2 };
        int[] presence = new int[43];

        for (int i = 0; i < data.length; i++) {
            presence[data[i]] = 1;
        }

        int target = 15;
        boolean exists = target > presence.length && presence[target] == 1;
        System.out.println("Exists: " + exists); // true
    }
}
```

# Solution 2: Recherche binaire sur tableau trié

## Description
La recherche binaire est une méthode efficace pour trouver une valeur dans un tableau trié. Elle consiste à diviser le tableau en deux à chaque étape et à comparer la valeur cible avec l’élément central.

## Avantages
- Complexité en temps : O(log n)
- Très rapide pour les grands tableaux
- Facile à implémenter

## Inconvénients
- Nécessite que le tableau soit trié
- Moins flexible si les données changent fréquemment

## Solution
```java
public class RechercheBinaire {
    public static boolean search(int[] data, int cible) {
        int gauche = 0;
        int droite = data.length - 1;
        while (gauche <= droite) {
            int milieu = (gauche + droite) / 2;
            if (data[milieu] == cible) {
                return true;
            } 
            if (data[milieu] < cible) {
                gauche = milieu + 1;
            } else {
                 droite = milieu - 1;
            }
        }
        return false;
    }

    public static void main(String[] args) {
        int[] ids = { 24042, 24017, 24008, 24023, 24004, 24016, 24015, 24009, 24030, 24002 };
        System.out.println(search(data, 24015)); // true
    }
}
```
