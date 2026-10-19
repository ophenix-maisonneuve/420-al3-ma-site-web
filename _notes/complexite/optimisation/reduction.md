---
layout: default
parent: "Optimisation"
title: "Réduction du problème"
nav_order: 3
published: true
---


# Réduction du problème

## Objectif
Reformuler un problème complexe en sous-problèmes plus simples.

## Explication
Réduire un problème permet de mieux structurer la solution et de préparer l’utilisation de l'une des autres méthodes sur chacun des sous-problèmes:

- Structure de données mieux adaptée
- Algorithme plus performant
- Programmation dynamique (mémoïsation ou tabulation)

## Exemple

On considère une grille de taille **m × n**. L’objectif est de déterminer le nombre de chemins possibles pour aller du coin supérieur gauche au coin inférieur droit, en ne se déplaçant que vers la droite ou vers le bas.

À première vue, on peut modéliser ce problème comme un cas classique de récursivité :

Pour atteindre une case (i, j), il faut venir soit de la case à gauche (i, j-1), soit de la case au-dessus (i-1, j).
On additionne donc le nombre de chemins pour ces deux positions :
chemins(i,j)=chemins(i−1,j)+chemins(i,j−1)\text{chemins}(i,j) = \text{chemins}(i-1,j) + \text{chemins}(i,j-1)chemins(i,j)=chemins(i−1,j)+chemins(i,j−1)


Limite : 


- À première vue, ce problème peut être modélisé comme un cas classique de récursivité. Pour atteindre une case **(i, j)**, il faut arriver soit de la case à gauche (i, j-1), soit de la case au-dessus (i-1, j). On peut donc appeler la fonction récursive de calcul pour la case (**n-1, m-1**). Cette approche entraîne beaucoup de recalculs inutiles, car les mêmes sous-problèmes sont résolus plusieurs fois.
- En analysant le problème de manière plus approfondie, on peut s'apercevoir que le nombre de chemins pour atteindre une case donnée est fixe, et peut donc être calculé une seule fois. Cette réduction du problème nous permet de calculer une valeur pour chaque case **une seule fois**. Pour toute case dans la grille, le nombre de chemins différents possibles sera égal à la somme des chemins pour atteindre la case à sa **gauche** et celle **au-dessus**.

{: .astuce}
Une fois le problème réduit, certaines solutions peuvent apparaître clairement. Dans le cas présent, la réduction du problème nous montre que la programmation dynamique sous forme de tabulation serait très appropriée.

### Avant (calcul récursif du nombre de chemins dans une grille)
```java
public class GridPaths {
    public static int countPaths(int m, int n) {
        if (m == 1 || n == 1) {
             return 1;
        }
        return countPaths(m - 1, n) + countPaths(m, n - 1);
    }
}
```

### Après (réduction en tableau 2D)
```java
public class GridPaths {
    public static int countPaths(int m, int n) {
        int[][] dp = new int[m][n];
        for (int i = 0; i < m; i++) {
             dp[i][0] = 1;
        }
        for (int j = 0; j < n; j++) {
             dp[0][j] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
            }
        }
        return dp[m - 1][n - 1];
    }
}
```
