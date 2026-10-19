---
layout: default
title: "Structures linéaires"
parent: "Structures de données"
nav_order: 1
published: true
---

# Structures linéaires

En programmation orientée objet (POO), une **structure linéaire** est une structure de données qui permet de stocker une collection séquentielle et ordonnée d'objets. Chaque élément de la liste peut être accédé par son index, et la liste peut parfois, selon les implémentations, contenir des objets de types différents. Cependant, dans la pratique, les éléments sont très souvent du même type.

## Caractéristiques principales
- **Ordonnée** : les éléments sont stockés dans un ordre précis.
- **Indexée** : chaque élément peut être accédé via son index (même si l'accès n'est pas toujours direct)
- **Mutable** : les listes peuvent être modifiées après leur création.

## Types d'opérations principales
- **Ajout** : insérer un élément à une position donnée.
- **Suppression** : retirer un élément spécifique ou à une position donnée.
- **Recherche** : localiser un élément dans la structure.
- **Tri** : organiser les éléments selon un ordre défini.

{: .highlight}
> En Java, les structures linéaires (sauf le tableau simple) sont représentés via l'interface `List` et ses implémentations comme `ArrayList`, `LinkedList` et `CopyOnWriteArrayList`. Voir [Interface List](../notes/interface-list) pour plus de détails.

--- 

## Types de structures linéaires

Une structure linéaire peut, dans sa forme la plus simple, être représentée par un tableau. Il existe aussi des versions plus dynamiques appelées listes chaînées, qui peuvent être simples (chaînées dans un seul sens) ou doublement chaînées (navigables dans les deux sens), ainsi que d'autres implémentations.

| Type de liste            | Taille dynamique | Accès direct | Insertion rapide | Suppression rapide | Complexité mémoire | Implémentation Java |
|--------------------------|------------------|--------------|------------------|--------------------|--------------------|
| Tableau                  | ❌               | ✅           | ❌               | ❌                 | Faible             | int[] |
| Liste chaînée            | ✅               | ❌           | ✅               | ✅                 | Élevée             | LinkedList |
| ArrayList                | ✅ (redimensionnement automatique) | ✅  | ❌ | ❌ | Faible | ArrayList


---

## Opérations courantes sur les structures linéaires

| **Algorithme** | **Fonctionnalité**            | **Java**                     | **Python**               | **Description** |
|----------------|-------------------------------|------------------------------|--------------------------|-----------------|
| Ajout          | Ajouter un élément            | `add(objet)`                 | `append(objet)`          | Ajoute un élément à la fin de la liste. |
| Ajout          | Insérer à une position        | `add(index, objet)`          | `insert(index, objet)`   | Insère un élément à une position donnée. |
| Suppression    | Supprimer par index           | `remove(index)`              | `pop(index)`             | Supprime l’élément à l’index spécifié. |
| Tri            | Trier la liste                | `Collections.sort(liste)`    | `sort()`                 | Trie les éléments de la liste. |
| Recherche      | Vérifier la présence          | `contains(objet)`            | `objet in liste`         | Vérifie si un élément est présent dans la liste. |
| Recherche      | Trouver l’index d’un élément  | `indexOf(objet)`             | `index(objet)`           | Retourne l’index de la première occurrence. |
| Recherche & suppression | Supprimer par valeur          | `remove(Object)`             | `remove(objet)`          | Supprime la première occurrence d’un élément. |
| Accès direct ou recherche <sup>1</sup> | Accéder par index             | `get(index)`              | `list[index]`             | Accède à l'élément à une position donnée sans effectuer de recherche. |

<sup>1</sup> *Cela dépend du type de liste. Pour une liste indexée, c'est-à-dire une liste où l'on peut accéder à un élément car son index (un tableau, par exemple), l'accès est direct. Pour une liste sans index, comme une liste chaînée classique, l'opération s'apparente plutôt à une opération de recherche linéaire, car on doit itérer sur tous les éléments de la liste jusqu'à l'index désiré.*

---

## Complexité des algorithmes sur les structures linéaires

Voici une comparaison sommaire de la complexité des opérations fondamentales (ajout, suppression, recherche, tri) selon les principales structures de données linéaires.

<details markdown="1">
<summary markdown="span">**Ajout**</summary>

| Structure              | Ajout en tête | Ajout en fin                        | Ajout à une position |
|------------------------|---------------|-------------------------------------|-----------------------|
| Tableau                | O(n)          | O(1) ou O(n) si tableau plein       | O(n)                  |
| Liste chaînée simple   | O(1)          | O(n) ou O(1) si *tail* maintenu     | O(n)                  |
| Liste chaînée double   | O(1)          | O(n) ou O(1) si *tail* maintenu     | O(n)                  |

{: .highlight}
> Dans un tableau, l’ajout en fin est O(1) si la capacité est suffisante. Sinon, il faut créer un nouveau tableau plus grand et copier les éléments, ce qui donne une complexité O(n).
> Dans une liste chaînée (simple ou double), l'ajout en fin est O(n), à moins qu'une référence vers le dernier noeud (*tail*) soit conservée.
</details>

<details markdown="1">
<summary markdown="span">**Suppression**</summary>

| Structure              | Suppression en tête | Suppression en fin                  | Suppression à une position |
|------------------------|----------------------|-------------------------------------|-----------------------------|
| Tableau                | O(n)                 | O(1) ou O(n) selon l’implémentation | O(n)                        |
| Liste chaînée simple   | O(1)                 | O(n)                                | O(n)                        |
| Liste chaînée double   | O(1)                 | O(1) si *tail* maintenu             | O(n)                        |

{: .highlight}
> Dans un tableau, cela dépend de si on accepte d'avoir des éléments vides, sans quoi il faut recopier le tableau (O(n))
> Dans une liste chaînée simple, même avec une référence à *tail*, la suppression en fin reste O(n) car il faut accéder à l’avant-dernier noeud.
> Dans une liste doublement chaînée avec *tail*, on peut accéder directement au prédécesseur du dernier noeud, ce qui permet une suppression en O(1).
</details>

<details markdown="1">
<summary markdown="span">**Recherche**</summary>

| Méthode             | Complexité moyenne | Complexité pire cas | Structure requise               | Avantages                          | Limitations                          |
|---------------------|--------------------|----------------------|----------------------------------|-------------------------------------|--------------------------------------|
| Recherche linéaire  | O(n)               | O(n)                 | Toutes                          | Simple, universelle                | Peu efficace sur grandes structures  |
| Recherche binaire   | O(log n)           | O(log n)             | Liste triée avec accès direct   | Très rapide sur grands ensembles   | Nécessite tri et accès par index     |

{: .highlight}
> La recherche binaire nécessite un accès direct aux éléments par leur index, ce qui n’est pas possible dans une liste chaînée. La recherche linéaire reste donc la méthode la plus adaptée pour les listes chaînées.
</details>

<details markdown="1">
<summary markdown="span">**Tri**</summary>

| Algorithme de tri   | Complexité moyenne | Complexité pire cas | Remarques                         |
|---------------------|--------------------|---------------------|----------------------------------|
| Tri par insertion   | O(n²)              | O(n²)               | Efficace sur petites listes      |
| Tri à bulles        | O(n²)              | O(n²)               | Simple mais peu performant       |
| Tri rapide (quicksort)| O(n log n)       | O(n²)               | Très rapide en pratique          |
| Tri fusion (merge sort)         | O(n log n)         | O(n log n)          | Souvent préféré sur les listes chaînées  |

</details>

---

## Piles et files
Les piles (*LIFO : Last In First Out*) et les files (prioritaires ou *FIFO : First In First Out*) sont également des structures linéaires. Elles s'appuient sur des tableaux ou des listes chaînées pour leur implémentation mais, contrairement aux listes énumérées ci-haut, elles ne permettent généralement pas l'accès à tous les éléments. Elle permettent plutôt de récupérer le prochain élément à sortir de la pile ou de la file, ce qui varie selon la politique d'accès définie par l'implémentation (FIFO, LIFO, priorité).

{: .highlight}
> En Java, elles sont représentées par les interfaces `Queue` et `Deque`, avec des implémentations comme `ArrayDeque`, `PriorityQueue` et `ConcurrentLinkedDeque`. L'interface `Deque`, qui signifie *Double-Ended Queue*, permet d’ajouter et de retirer des éléments aux deux extrémités, ce qui la rend utilisable comme pile ou comme file.