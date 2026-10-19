---
layout: default
title: "Arbre binaire de recherche (BST)"
nav_order: 7
has_toc: false
published: true
---

# Exercice : Implémentation d'un arbre binaire de recherche (BST)

## Objectifs

- Comprendre la structure d'un arbre binaire de recherche.
- Implémenter des opérations courantes sur les arbres : insertion, suppression, recherche, parcours.
- Analyser la complexité des opérations et proposer des améliorations.

## Contexte

On vous fournit une classe `ArbreBinaireRecherche` dans laquelle les méthodes suivantes sont déjà incluses (les autres devront être implémentées):
- `void inserer(int valeur)` : Insère une valeur dans l'arbre.
- `String toString()` : Fournit une version lisible du contenu de l'arbre (par exemple, parcours infixe).

On vous fournit également une classe `Main` contenant un menu utilitaire permettant d'invoquer les différentes actions sur l'arbre.

## Étapes préparatoires

### 1. Clonez le dépôt de l'exercice
```bash
git clone git@github.com:ophenix-420-930-ma-24636/arbre-binaire-recherche.git
```
ou
```bash
git clone https://github.com/ophenix-420-930-ma-24636/arbre-binaire-recherche.git
```

### 2. Lancez le projet Java
```bash
mvn clean package
java -jar target/arbre-binaire-recherche-1.0-SNAPSHOT.jar
```
ou
```bash
mvn clean compile exec:java
```
ou directement à partir de votre IDE.

Familiarisez-vous avec le menu, qui vous permet déjà d'ajouter des éléments dans l'arbre.

## Questions

### 1. Étudiez les classes `Noeud` et `ArbreBinaireRecherche`.
- Quelle est la structure de l'arbre ? (binaire, BST ?)
- Qu'est-ce qui vous a permis de l'identifier ?

### 2. Étudiez la classe `ArbreBinaireRecherche`.
- De quelle manière l'application vous permet déjà d'ajouter des éléments dans l'arbre ?
- Quelle est la complexité grand O de l'insertion dans le meilleur cas et dans le pire cas ?
- Que se passe-t-il si l'arbre est très déséquilibré ?

### 3. Implémentez la recherche dans l'arbre.
- Implémentez la méthode `contient(int valeur)` pour déterminer si une valeur est présente ou non dans l'arbre.
- Quelle est la complexité grand O de cette opération dans le pire cas ?
- Comment peut-on s'assurer que la complexité n'augmente pas significativement ?

### 4. Implémentez le parcours de l'arbre.
- Implémentez la méthode `parcoursInfixe()`.
- Quelle est la complexité grand O de cette opération ?
- Ajoutez une méthode `parcoursInfixeInverse()` pour afficher les valeurs en ordre décroissant.

### 5. Implémentez la suppression dans l'arbre.
- Implémentez la méthode `supprimer(int valeur)` pour supprimer un nœud dans l'arbre.
- Quels sont les cas particuliers à gérer (noeud sans enfant, avec un enfant, avec deux enfants) ?
- Quelle est la complexité grand O de cette opération dans le pire cas ?

### 6. Exercice Bonus : Implémentation des parcours préfixe et suffixe
- Implémentez les méthodes `parcoursPrefixe()`, et `parcoursSuffixe()`.
- Quelle différence remarquez-vous par rapport à l'ordre dans lequel les valeurs sont affichées ?
- À quoi servent les parcours préfixe et suffixe ?

{: .astuce}
> Petit lien utile (YouTube) : [Binary Search Tree Explained](https://www.youtube.com/watch?v=mtvbVLK5xDQ)

## Pistes de réflexion
- Comment pouvez-vous réutiliser cet exercice dans la réalisation du travail pratique 1 ?
- Quels ajouts seraient nécessaires pour gérer les doublons dans l'arbre ?
- Quels ajouts seraient nécessaires pour transformer cet arbre en arbre équilibré ?
