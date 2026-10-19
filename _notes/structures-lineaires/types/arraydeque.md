---
title: "java.util.ArrayDeque"
parent: "java.util.Deque"
layout: default
nav_order: 1
has_toc: false
published: true
---

# java.util.ArrayDeque

![Diagramme ArrayDeque](../assets/images/arraydeque.png)

## Description

**ArrayDeque** est une implémentation de **Deque** basée sur un tableau redimensionnable. Elle ne permet pas les éléments `null`.

## Fonctionnement interne

`ArrayDeque` repose sur un **tableau circulaire redimensionnable**. Les éléments sont stockés dans un tableau interne, et deux indices, *head* et *tail*, indiquent respectivement la position du premier élément et la prochaine position d’insertion (si on fonctionne en mode **file d'attente**). Lorsque l’on ajoute ou retire des éléments aux extrémités, ces indices sont déplacés en utilisant une opération modulo, ce qui permet de « boucler » dans le tableau sans déplacer tous les éléments. Ce mécanisme rend les opérations en tête et en queue très rapides (O(1) amorti).

Lorsque le tableau est plein, `ArrayDeque` crée un nouveau tableau de taille doublée, puis copie les éléments dans l’ordre logique (en commençant par head), afin de préserver la continuité circulaire. Ce processus est similaire au redimensionnement d’`ArrayList`, mais adapté à la structure circulaire.

## Forces et faiblesses

### Forces
- Opérations **rapides** en tête et en queue (O(1) amorti).
- Pas de limite de capacité (sauf mémoire).
- Légèrement plus rapide que `LinkedList` pour les opérations de pile/file (mais O(1) dans les deux cas).
- Pas de surcharge liée à la synchronisation (performant en contexte *mono-thread*).

### Faiblesses
- Non *thread-safe* (pas adaptée aux environnements concurrents).
- Ne permet pas les éléments `null`.
- Redimensionnement interne peut coûter cher si la taille augmente fortement.

## Quand l'utiliser ?
- Implémenter une **pile (LIFO)** pour gérer des opérations réversibles (ex. : annulation).
- Implémenter une **file (FIFO)** pour traiter des tâches dans l'ordre d'arrivée.
- Gestion de buffers temporaires.

## Complexité

| Opération | Complexité |
|-----------|------------|
| addFirst / addLast | O(1) amorti |
| removeFirst / removeLast | O(1) amorti |

## Exemple : pile (LIFO)

```java
Deque<String> pile = new ArrayDeque<>();
pile.push("A");
pile.push("B");
System.out.println(pile.pop()); // B
```

## Exemple : file (FIFO)
```java
Queue<String> file = new ArrayDeque<>();
file.add("A");
file.add("B");
System.out.println(file.remove()); // A
```

### À voir aussi
[Documentation Java ArrayDeque](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/ArrayDeque.html)
