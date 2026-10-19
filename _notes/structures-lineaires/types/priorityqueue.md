---
title: "java.util.PriorityQueue"
parent: "Structures linéaires"
layout: default
nav_order: 4
has_toc: false
published: true
---

# Classe `java.util.PriorityQueue`

## Description

**PriorityQueue** est une file qui ordonne ses éléments selon leur ordre naturel (s'ils implémentent `Comparable`) ou un `Comparator` fourni. Elle est implémentée avec un **tas binaire** (*binary heap*) stocké dans un tableau.


## Fonctionnement interne
 
Conceptuellement, un **tas binaire** (*binary heap*) est un arbre binaire complet qui obéit aux règles suivantes:
- Un parent ne peut avoir au maximum que 2 enfants
- La relation parent-enfant suit une logique particulière, propre à l'implémentation. Par exemple:
  - ***min-heap***: le parent est toujours plus petit que ses enfants
  - ***max-heap***: le parent est toujours plus grand que ses enfants

Dans la `PriorityQueue`, on utilise un *min-heap* implémenté à l'aide d'un tableau où le plus petit élément (ou le plus prioritaire selon le `Comparator`) se trouve toujours à la racine. Lorsqu’un élément est inséré, il est ajouté à la fin du tableau, puis « remonté » (*heapify up*) pour restaurer la propriété du tas. Lorsqu’on retire la tête (élément prioritaire), le dernier élément du tableau est déplacé à la racine, puis « descendu » (*heapify down*) pour rétablir l’ordre. Ces ajustements garantissent que la tête est toujours l’élément le plus prioritaire, avec des opérations d’insertion et de suppression en **O(log n)**. Ce fonctionnement est très efficace pour gérer des files où l’ordre de priorité est essentiel, mais il ne garantit pas un ordre global pour tous les éléments, seulement pour la tête.

### Règles d’indexation dans le tableau
Pour un élément à l’index `i` dans le tableau :
- **Parent** : `(i - 1) / 2` (division entière)
- **Enfant gauche** : `2 * i + 1`
- **Enfant droit** : `2 * i + 2`

### Exemple

#### Arbre
```
        1
      /   \
     3     2
    / \   / \
   7   6 4   5
```

#### Tableau correspondant
```
Index :  0  1  2  3  4  5  6
Valeur:  1  3  2  7  6  4  5
```

####  Vérification des relations
- Parent de `3` (index 1) = `(1 - 1) / 2 = 0` → `1`
- Enfant gauche de `2` (index 2) = `2 * 2 + 1 = 5` → `4`
- Enfant droit de `2` (index 2) = `2 * 2 + 2 = 6` → `5`

{: .highlight}
> Comme `PriorityQueue` ne garantit l'ordre que pour la tête (élément le plus prioritaire), elle ne peut être utilisée comme queue à deux extrémités (**deque**). Elle implémente donc directement l'interface `Queue`, et non l'interface `Deque`.

## Forces et faiblesses

### Forces

- Permet de gérer des éléments avec **priorité**.
- Opérations de suppression et insertion efficaces (O(log n)).
- Idéale pour les algorithmes de graphes (Dijkstra, A*).

### Faiblesses

- Pas thread-safe.
- Ne garantit pas l'ordre global (seule la tête est prioritaire).
- Ne permet pas les éléments `null`.

## Quand l'utiliser ?
- Gestion de tâches avec priorité.
- File où des éléments doivent être priorisés :
  - Joueurs VIP
  - Événements urgents
  - etc.

## Complexité

| Opération | Complexité |
|-----------|------------|
| insertion | O(log n) |
| suppression tête | O(log n) |

## Exemple
```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
pq.add(10);
pq.add(5);
pq.add(20);
System.out.println(pq.poll()); // 5
```

### À voir aussi
[Documentation Java PriorityQueue](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/PriorityQueue.html)
