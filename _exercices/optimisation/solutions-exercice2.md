---
layout: default
parent: "Optimisation d'algorithmes"
title: "Exercice 2 - Solution"
nav_order: 1
has_toc: false
---

# Solution 1: mémoïsation

## Principe
On utilise une structure (tableau) pour **mémoriser les résultats** déjà calculés afin d’éviter les appels récursifs redondants.

## Solution
```java
public class FibonacciMemo {
    public static int fib(int n, int[] memo) {
        if (memo[n] != -1) {
            return memo[n];
        }
        if (n <= 1) {
            memo[n] = n;
        } else {
            memo[n] = fib(n - 1, memo) + fib(n - 2, memo);
        }
        return memo[n];
    }

    public static void main(String[] args) {
        int n = 10;
        int[] memo = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            memo[i] = -1;
        } 
        System.out.println("Fibonacci(" + n + ") = " + fib(n, memo));
    }
}
```

## Avantages
- Temps d’exécution réduit à O(n)
- Facile à implémenter

## Inconvénients
- Utilise de la mémoire supplémentaire pour le tableau
- Toujours récursif (pile d’appels)


# Solution 2: tabulation

## Principe
On construit la solution **de bas en haut**, en remplissant un tableau avec les résultats intermédiaires.

## Solution
```java
public class FibonacciTab {
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
        int n = 10;
        System.out.println("Fibonacci(" + n + ") = " + fib(n));
    }
}
```

## Avantages
- Temps d’exécution O(n)
- Pas de récursion, donc pas de pile d’appels

## Inconvénients
- Utilise un tableau de taille n
- Possiblement moins intuitif