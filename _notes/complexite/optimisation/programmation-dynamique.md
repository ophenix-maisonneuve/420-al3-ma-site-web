---
layout: default
parent: "Optimisation"
title: "Programmation dynamique"
nav_order: 4
published: true
---

# Programmation dynamique

## Objectif
Optimiser les calculs en évitant les répétitions grâce à la mémorisation des résultats intermédiaires.

## Explication
La programmation dynamique est utile lorsque le problème peut être décomposé en sous-problèmes qui se répètent. Elle permet de réduire la complexité en stockant les résultats déjà calculés. Les deux méthodes les plus souvent utilisées sont:

- **Mémoïsation** (*top-down*) : on stocke les résultats dans un cache au fur et à mesure des appels récursifs.
- **Tabulation** (*bottom-up*) : on construit une table de résultats à partir des cas de base.

## Pourquoi c’est utile
- Réduction drastique du temps d’exécution
- Exploitation efficace des dépendances internes du problème
- Applicable à de nombreux problèmes combinatoires

{: .astuce}
>Si l'algorithme...
>
>- est récursif,
>- recalcule les mêmes choses plusieurs fois,
>- cherche à optimiser une valeur sous contrainte,
>- ou utilise une structure comme un tableau pour stocker des résultats,
>
>... alors la programmation dynamique (mémoïsation ou tabulation) pourrait considérablement améliorer ses performances.

---


## Exemple 1 : Suite de Fibonacci

### Énoncé 
La suite de Fibonacci est définie par :  
- `fib(0) = 0`, `fib(1) = 1`, et `fib(n) = fib(n-1) + fib(n-2)` pour `n >= 2`.
- Les premières valeurs de cette suite sont: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ...
- Le but est de calculer `fib(n)` efficacement.

### Solution récursive simple
La solution récursive simple recalculera plusieurs fois les mêmes valeurs, ce qui donne une complexité exponentielle O(2<sup>n</sup>).

<details markdown="1">
<summary markdown="span">Code Java</summary>

```java
public class Fibonacci {
    public static int fib(int n) {
        if (n <= 1) {
            return n;
        }
        return fib(n - 1) + fib(n - 2);
    }

    public static void main(String[] args) {
        int n = 10; // Exemple : calculer le 10e terme
        System.out.println("Fibonacci(" + n + ") = " + fib(n));
    }
}
```
</details>

{: .highlight}
> Les appels récursifs à `fib(n-1)` et `fib(n-2)` se chevauchent énormément, ce qui entraîne des recalculs inutiles. Ceci est un indice que la programmation dynamique pourrait être bénéfique...

### Optimisation par programmation dynamique

#### Mémoïsation (top-down) 
On utilise une fonction récursive avec un cache pour mémoriser les résultats déjà calculés.

<details markdown="1">
<summary markdown="span">Code Java</summary>

```java
public class Fibonacci {
    static int[] memo;

    public static int fib(int n) {
        if (memo[n] != -1) {
            return memo[n];
        } 
        if (n <= 1) {
             return n;
        }
        memo[n] = fib(n - 1) + fib(n - 2);
        return memo[n];
    }

    public static void main(String[] args) {
        int n = 10;
        memo = new int[n + 1];
        for (int i = 0; i <= n; i++) {
             memo[i] = -1;
        }
        System.out.println(fib(n));
    }
}
```
</details>

#### Tabulation (bottom-up) 
On construit un tableau `fib[]` en partant des cas de base et en calculant chaque valeur jusqu’à `n`.

<details markdown="1">
<summary markdown="span">Code Java</summary>

```java
public class Fibonacci {
    public static int fib(int n) {
        if (n <= 1) {
            return n;
        }

        int[] dp = new int[n + 1];
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }

    public static void main(String[] args) {
        System.out.println(fib(10));
    }
}
```
</details>

### Complexité optimisée
- Temps : O(n)
- Espace : O(n) (ou O(1) avec optimisation)

{: .highlight}
> Ce problème est un excellent candidat pour la programmation dynamique car :
> - Il est naturellement récursif.
> - Il présente des sous-problèmes qui se répètent.
> - Il peut être résolu efficacement en mémorisant les résultats intermédiaires.

---

## Exemple 2 : Problème du sac à dos (*Knapsack*)

### Énoncé  
- On dispose d’un sac à dos pouvant contenir un poids maximal `W`.  
- On a `n` objets, chacun avec un poids `w[i]` et une valeur `v[i]`.  
- Le but est de **maximiser la valeur totale** des objets placés dans le sac **sans dépasser le poids `W`**.

### Solution récursive de base
On peut essayer toutes les combinaisons possibles d’objets, ce qui donne une complexité exponentielle O(2<sup>n</sup>).

<details markdown="1">
<summary markdown="span">Code Java</summary>

```java
public class KnapsackNaive {
    public static int knapsack(int[] weights, int[] values, int W, int n) {
        // Cas de base : plus d'objets ou plus de capacité
        if (n == 0 || W == 0) {
             return 0;
        }

        // Si le poids de l'objet est trop grand, on ne peut pas le prendre
        if (weights[n - 1] > W) {
            return knapsack(weights, values, W, n - 1);
        } else {
            // Deux choix : inclure ou exclure l'objet
            int include = values[n - 1] + knapsack(weights, values, W - weights[n - 1], n - 1);
            int exclude = knapsack(weights, values, W, n - 1);
            return Math.max(include, exclude);
        }
    }

    public static void main(String[] args) {
        int[] weights = {1, 3, 4, 5};
        int[] values = {1, 4, 5, 7};
        int W = 7;
        int n = weights.length;

        System.out.println("Valeur maximale (naïve) : " + knapsack(weights, values, W, n));
    }
}
```
</details>

{: .highlight}
> De nombreux sous-problèmes sont recalculés : par exemple, la meilleure combinaison pour un poids `W'` avec les `i` premiers objets. Ceci est un indice que la programmation dynamique pourrait être bénéfique...


### Optimisation par programmation dynamique

#### Mémoïsation (top-down)  
On utilise une fonction récursive avec un cache pour mémoriser les résultats déjà calculés pour `(i, w)`.

<details markdown="1">
<summary markdown="span">Code Java</summary>

```java
import java.util.HashMap;
import java.util.Map;

public class KnapsackMemo {
    static Map<String, Integer> memo = new HashMap<>();

    public static int knapsack(int[] weights, int[] values, int W, int n) {
        if (n == 0 || w == 0) {
             return 0;
        }

        String key = n + "-" + W;
        if (memo.containsKey(key)) {
            return memo.get(key);
        } 

        if (weights[n - 1] > W) {
            memo.put(key, knapsack(weights, values, W, n - 1));
        } else {
            int include = values[n - 1] + knapsack(weights, values, W - weights[n - 1], n - 1);
            int exclude = knapsack(weights, values, W, n - 1);
            memo.put(key, Math.max(include, exclude));
        }

        return memo.get(key);
    }

    public static void main(String[] args) {
        int[] weights = {1, 3, 4, 5};
        int[] values = {1, 4, 5, 7};
        int W = 7;
        int n = weights.length;

        System.out.println("Valeur maximale (mémoïsation) : " + knapsack(weights, values, w, n));
    }
}
```
</details>

#### Tabulation (bottom-up)  
On construit une table `dp[i][w]` où chaque cellule contient la valeur maximale atteignable avec les `i` premiers objets et un poids `w`.

<details markdown="1">
<summary markdown="span">Code Java</summary>

```java
public class KnapsackTabulation {
    public static int knapsack(int[] weights, int[] values, int W) {
        int n = weights.length;
        int[][] dp = new int[n + 1][W + 1];

        // Remplir la table dp
        for (int i = 0; i <= n; i++) {
            for (int w = 0; w <= W; w++) {
                if (i == 0 || w == 0) {
                    dp[i][w] = 0; // Cas de base : pas d'objet ou capacité nulle
                } else if (weights[i - 1] <= w) {
                    // Choix : inclure ou exclure l'objet
                    dp[i][w] = Math.max(
                        values[i - 1] + dp[i - 1][w - weights[i - 1]],
                        dp[i - 1][w]
                    );
                } else {
                    // Objet trop lourd, on l'exclut
                    dp[i][w] = dp[i - 1][w];
                }
            }
        }

        return dp[n][W]; // Valeur maximale atteignable
    }

    public static void main(String[] args) {
        int[] weights = {1, 3, 4, 5};
        int[] values = {1, 4, 5, 7};
        int W = 7;

        System.out.println("Valeur maximale (tabulation) : " + knapsack(weights, values, W));
    }
}
```
</details>

### Complexité optimisée
- Temps : O(n \* W)
- Espace : O(n \* W) (ou O(W) avec optimisation)

{: .highlight}
> Ce problème est un excellent candidat à la programmation dynamique car :
> - Il a une structure récursive.
> - Il présente des sous-problèmes qui se répètent.
> - Il cherche à optimiser une valeur sous contrainte.
