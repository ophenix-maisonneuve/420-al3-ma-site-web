---
layout: default
title: "Notation Grand O"
parent: "Complexité des algorithmes"
nav_order: 1
published: true
---

# Notation Grand O

## Définition

La **notation Grand O** (ou Big O) est une mesure standard en informatique pour analyser la performance des algorithmes. Elle permet de caractériser la manière dont le temps d’exécution ou la consommation de mémoire évolue en fonction de la taille des données. Cette approche est devenue essentielle pour comparer des solutions et optimiser le code, car elle donne une vision claire de la capacité de montée en charge (*scalability*) d’une application.

Contrairement à une mesure exacte, la notation Grand O ne cherche pas à fournir une équation précise pour un algorithme donné. Elle s’intéresse à l’**ordre de grandeur** général, ce qui permet d’ignorer les constantes et les facteurs secondaires qui influencent peu la performance à grande échelle. En se concentrant sur la tendance dominante, on peut anticiper les comportements pour des volumes de données importants et choisir des algorithmes adaptés, garantissant ainsi des systèmes plus efficaces et robustes.

{: .highlight}
>En bref, on cherche à:
>- **Anticiper la croissance** : La notation Grand O sert à estimer, avant même d’exécuter le programme, comment le temps d’exécution (ou la mémoire utilisée) évolue lorsque la taille des données d’entrée, souvent notée n, augmente. Cela nous aide à prévoir si un algorithme restera rapide ou deviendra très coûteux avec des données plus grandes.
>- **Ignorer les détails secondaires** : On ne tient pas compte des constantes ou des petits ajustements. Par exemple, un algorithme qui fait **3n** opérations et un autre qui en fait **5n** sont tous les deux considérés comme **O(n)**, car ce qui compte, c’est la tendance générale, pas la différence de quelques opérations.
>- **Caractériser la tendance** : L’objectif est de décrire le comportement global : est-ce que l’algorithme reste constant (O(1)), croît proportionnellement (O(n)), très rapidement (O(n<sup>2</sup>)), ou explose de manière exponentielle (O(2<sup>n</sup>)) ? Cette classification permet de comparer les algorithmes et de choisir les plus adaptés pour des volumes de données importants.

## Principales complexités

| Complexité   | Nom commun         | Exemple typique                     |
|--------------|--------------------|-------------------------------------|
| O(1)         | Constante          | Accès direct à un élément           |
| O(log n)     | Logarithmique      | Recherche binaire (dichotomique)    |
| O(n)         | Linéaire           | Parcours d’un tableau               |
| O(n log n)   | Linéarithmique     | Tri rapide (quicksort)              |
| O(n²)        | Quadratique        | Comparaison de tous les couples     |
| O(2ⁿ)        | Exponentielle      | Algorithme récursif non optimisé    |

## Impact des structures de code sur la complexité

Voici comment certaines structures influencent généralement la complexité d’un algorithme :

- **Conditions (`if`)** :
  - N’ajoutent généralement pas de complexité, sauf si elles contiennent des appels coûteux.
  - Exemple : `if (x > 0) { ... }` est O(1), mais si le bloc contient une boucle, la complexité dépend de cette boucle.

- **Boucles (`for`, `while`)** :
  - La complexité dépend du nombre d’itérations.
  - Une boucle simple sur `n` éléments est O(n).
  - Une boucle imbriquée peut être O(n<sup>2</sup>) ou plus.

- **Appels simples de méthode** :
  - Si la méthode est O(1), l’appel est O(1).
  - Si elle contient une boucle, la complexité dépend de son contenu.

- **Appels récursifs** :
  - Peuvent rapidement augmenter la complexité.
  - Exemple : une fonction récursive qui appelle deux fois elle-même à chaque étape peut être O(2<sup>n</sup>).