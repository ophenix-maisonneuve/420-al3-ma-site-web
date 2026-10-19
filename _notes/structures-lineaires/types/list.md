---
title: "java.util.List"
parent: "Structures linéaires"
layout: default
nav_order: 3
has_toc: true
published: true
---

# Interface `java.util.List`

## Description

L'interface **List** en Java représente une collection ordonnée qui permet d’accéder aux éléments par leur position (index).Elle étend l'interface **Collection**, et plus spécifiquement l'interface **SequencedCollection**. Elle fournit des méthodes pour accéder aux éléments par index, insérer, supprimer, rechercher et parcourir.

## Quand l'utiliser ?

Une **List** est idéale lorsque l’ordre des éléments est important ou lorsque des opérations fréquentes d’insertion, de suppression ou de parcours séquentiel sont nécessaires. Contrairement à un **Set**, une liste autorise les doublons et conserve l’ordre d’insertion. Elle se distingue également d’une **Map**, qui associe des clés à des valeurs, car une liste ne gère que des éléments simples sans clé explicite.

### Scénarios d'utilisation propices

- **Maintenir un ordre précis** : lorsque l’ordre d’insertion ou un ordre logique est essentiel (ex. : étapes d’un processus).
- **Accès par position** : besoin de récupérer ou modifier un élément à un index spécifique.
- **Présence de doublons** : lorsque des éléments identiques doivent être autorisés (ex. : liste de participants où un nom peut apparaître plusieurs fois).
- **Parcours séquentiel** : itération dans un ordre déterminé (ex. : affichage chronologique).
- **Insertion et suppression dynamiques** : ajout ou retrait d’éléments à des positions spécifiques.
- **Collections de taille variable** : lorsque la taille n’est pas fixe et évolue au cours du programme.
- **Traitement ordonné avant tri** : préparation d’une liste pour appliquer un tri personnalisé ou naturel.

---

## Méthodes principales

| **Méthode** | **Description** |
|-------------|------------------|
| [add(E e)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/List.html#add(E)) | Ajoute un élément à la fin de la liste. |
| [add(int index, E element)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/List.html#add(int,E)) | Ajoute un élément à la position spécifiée (décale les éléments suivants). |
| [set(int index, E element)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/List.html#set(int,E)) | Remplace l’élément à l’index donné par un nouvel élément. |
| [remove(int index)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/List.html#remove(int)) | Supprime l’élément à la position spécifiée. |
| [remove(Object o)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/List.html#remove(int)) | Supprime la première occurrence de l’objet spécifié (si présent). |
| [clear()](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/List.html#clear()) | Supprime tous les éléments de la liste. |
| [get(int index)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/List.html#get(int)) | Retourne l’élément à la position spécifiée. |
| [contains(Object o)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/List.html#contains(java.lang.Object)) | Effectue une recherche **linéaire** et retourne `true` si l'objet recherché a été trouvé. |
| [indexOf(Object o)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/List.html#indexOf(java.lang.Object)) | Effectue une recherche **linéaire** et retourne l'index de la première occurrence de l’objet (ou -1 si absent). |
| [sort(Comparator<? super E> c)](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/List.html#sort(java.util.Comparator)) | Trie la liste selon le comparateur fourni ; si `null`, utilise l’ordre naturel. |
| [reversed()](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/List.html#reversed()) | Retourne une vue inversée de la liste. Cette méthode **n'inverse pas réellement la liste sous-jacente**. |
| [size()](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/List.html#size()) | Retourne le nombre d’éléments dans la liste. |


### En résumé

- Les méthodes `add`, `remove` et `set` modifient la liste.  
- Les méthodes `get`, `contains`, `indexOf` et `size` sont des opérations de lecture.  
- `sort` est utile pour organiser les éléments selon un ordre spécifique.
- `reversed` fournit une vue (*view*) inversée de la collection.

{: .highlight}
> Afin de permettre une réutilisation maximale, les Collections Java font beaucoup usage des types génériques. Pour plus d'information sur ce sujet, consultez la page suivante: [Génériques en Java](../notes/generiques-java)


## Utilitaires


## Utilitaires

| Méthode | Description |
|---------|-------------|
| `Collections.reverse(list)` | Inverse l’ordre des éléments dans la liste spécifiée. |
| `Collections.shuffle(list)` | Mélange aléatoirement les éléments de la liste. |
| `Collections.binarySearch(List<? extends Comparable<? super T>> list, T key)` | Effectue une recherche dichotomique dans une liste triée pour trouver la position de `key`. |
| `Collections.synchronizedList(list)` | Retourne une vue synchronisée (thread-safe) de la liste spécifiée en utilisant un verrou global. |
| `Collections.unmodifiableList(list)` | Retourne une vue non modifiable de la liste spécifiée (toute tentative de modification déclenche une exception). |
| `Arrays.asList(T... a)` | Crée une liste fixe (taille immuable) à partir d’un tableau ou d’éléments passés en paramètres. |

{: .highlight}
> Il ne s'agit pas d'une liste exhaustive des méthodes utilitaires disponibles. Pour tous les détails, consultez [java.util.Collections](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/Collections.html).
