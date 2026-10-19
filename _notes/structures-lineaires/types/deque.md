---
title: "java.util.Deque"
parent: "Structures linéaires"
layout: default
nav_order: 4
has_toc: true
published: true
---

# Interface `java.util.Deque`

## Description
Les piles (*LIFO : Last In First Out*) et les files (*FIFO : First In First Out*, ou prioritaires) sont des structures linéaires spécialisées. Elles s'appuient sur des tableaux ou des listes chaînées pour leur implémentation mais, contrairement aux listes, elles ne permettent généralement pas l'accès direct à tous les éléments. Elles permettent plutôt de récupérer le prochain élément à sortir, selon la **politique d'accès** définie par l'implémentation (FIFO, LIFO, priorité).

L'interface **Deque** (Double-Ended Queue) permet d'ajouter et de retirer des éléments aux deux extrémités. Elle peut donc être utilisée comme :
- **Pile (LIFO)** : opérations sur la même extrémité.
- **File (FIFO)** : opérations sur des extrémités opposées.

Elle est une sous-interface de `java.util.Queue`, et peut donc aussi être utilisée exclusivement comme une file simple.


## Quand l'utiliser ?
- Pour implémenter des piles modernes (remplace `Stack`).
- Pour des files flexibles.

## Méthodes principales
Il est possible d'utiliser `Deque` en tant que queue à deux extrémités, mais également exclusivement en tant que **file** et en tant que **pile**. `Deque` définit donc plusieurs méthodes, dont certaines sont équivalentes, simplement pour avoir des noms de méthodes cohérents avec les opérations sur les **deque**, **file** et **pile**.

### Utilisation en tant que file à deux extrémités (*deque*)

| Méthode | Description |
|---------|-------------|
| addFirst(E e) | Ajoute en tête. |
| addLast(E e) | Ajoute en queue. |
| removeFirst() | Supprime l'élément de tête et le retourne|
| removeLast() | Supprime l'élément de queue et le retourne |
| getFirst() | Retourne le premier élément sans le supprimer|
| getLast() | Retourne le dernier élément sans le supprimer |


### Utilisation en tant que file (*queue*)

Une **file** est une structure linéaire où seule la manipulation de l’élément **situé en tête** (défilement) et en **queue** (insertion) est possible. On l’appelle aussi souvent *queue* ou *FIFO (First In, First Out)* en anglais. Il existe également d'autres types de files comme la **file prioritaire**, qui ne respecte pas l'ordre d'arrivée mais modifie constamment l'ordre pour faire passer les éléments les plus prioritaires en tête de file.


| Méthode | Description | Méthode équivalente |
|---------|-------------| --------------------
| add(E e) | Ajoute à la fin de la queue. | addLast(E e) |
| remove() | Supprime l'élément de tête et le retourne | removeFirst() |
| element() | Retourne l'élément de tête sans le supprimer | getFirst() |

{: .astuce}
> Comme l'interface `Deque` est une sous-interface de `Queue`, il est possible d'utiliser l'objet par son interface `Queue`, rendant l'utilisation encore plus facile. Par exemple:
> ```java
> Queue<Object> q = new ArrayDeque<>();
> ```

### Utilisation en tant que pile (*stack*)

Une **pile** est une structure linéaire où seule la manipulation de l’élément situé au **sommet** est possible (index 0 ou *head*). On l’appelle aussi souvent *stack* ou *LIFO (Last In, First Out) en anglais*.

| Méthode | Description | Méthode équivalente |
|---------|-------------| --------------------
| push(E e) | Ajoute sur la pile. | addFirst(E e) |
| pop() | Supprime et retourne la tête de la pile. | removeFirst() |
| peek() | Retourne la tête de la pile sans supprimer l'élément | peekFirst() |

{: .warning}
> L'interface `Deque` n'est **pas** une sous-interface de `Stack` (qui est une interface *legacy* et déconseillée aujourd'hui). Cependant, elle fournit des méthodes avec les mêmes noms que les méthodes de `Stack` pour faciliter l'utilisation: `push`, `pop` et `peek`.

### À voir aussi

[Documentation Java Deque](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/Deque.html)
