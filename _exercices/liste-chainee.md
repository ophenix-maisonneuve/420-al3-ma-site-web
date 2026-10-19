---
layout: default
title: "Liste Chaînée"
nav_order: 6
has_toc: false
published: true
---
# Exercice : Implémentation d'une liste chaînée


## Objectifs

- Comprendre la structure d'une liste chaînée et doublement chaînée.
- Comprendre et implémenter des opérations courantes sur les listes: ajout, suppression, recherche, tri.
- Proposer des améliorations pour l'ajout, la suppression et le tri.

## Contexte

On vous fournit une classe `ListeChainee` dans laquelle les méthodes suivantes sont déjà incluses (les autres devront être implémentées):
- `void ajouter(int valeur, int position)` : Ajoute une valeur à la position spécifiée dans la liste.
- `String toString()` : Fournit une version lisible du contenu de la liste en une seule chaîne de caractères.

On vous fournit également une classe `Main` contenant un menu utilitaire permettant d'invoquer les différentes actions sur la liste.

## Étapes préparatoires

### 1. Clonez le dépôt de l'exercice

```bash
git clone git@github.com:ophenix-420-930-ma-24636/liste-chainee.git
```
ou
```bash
git clone https://github.com/ophenix-420-930-ma-24636/liste-chainee.git
```

### 2. Lancez le projet Java
```bash
mvn clean package
java -jar target/liste-chainee-1.0-SNAPSHOT.jar
```
ou
```bash
mvn clean compile exec:java
```
ou directement à partir de votre IDE.

Familiarisez-vous avec le menu, qui vous permet déjà d'ajouter des éléments dans la liste.

## Questions

### 1. Étudiez les classes `Noeud` et `ListeChainee`.
- S'agit-il d'une liste chaînée simple ou double ?
- Qu'est-ce qui vous a permis de l'identifier ? 

### 2. Étudiez la classe `ListeChainee`.
- De quelle manière l'application vous permet déjà d'ajouter des éléments dans la liste ?
- Dans quel(s) cas l'ajout est-il le plus rapide ? Pourquoi ?
- Quelle est la complexité grand O de l'ajout dans le pire cas ?

### 3. Implémentez la recherche dans la liste.
- Implémentez la méthode `contient(int valeur)` pour déterminer si une valeur est présente ou non dans la liste.
- Quelle est la complexité grand O de cette opération dans le pire cas ?
- La complexité peut-elle être optimisée ? Pourquoi ?

### 4. Implémentez la suppression dans la liste.
- Implémentez la méthode `supprimer(int position)` pour supprimer un élément dans la liste.
- Dans quel(s) cas la  suppression est-elle la plus rapide ? Pourquoi ?
- Quelle est la complexité grand O de cette opération dans le pire cas ?

### 5. Implémentez un tri à bulle dans la liste.
- Implémentez la méthode `triABulle()` pour trier la liste.
- Quelle est la complexité grand O de cette opération dans le pire cas ?
- La complexité peut-elle être optimisée ? Comment ?

{: .astuce}
> Petit lien utile (YouTube) : [Bubble Sort in 2](https://www.youtube.com/watch?v=xli_FI7CuzA)

### 6. Exercice Bonus: Implémentez un tri par fusion dans la liste.
- Implémentez la méthode `triParFusion()` pour trier la liste.
- Quelle est la complexité grand O de cette opération dans le pire cas ?

{: .astuce}
> Petit lien utile (YouTube) : [Merge Sort in 3](https://www.youtube.com/watch?v=4VqmGXwpLqc)

## Pistes de réflexion
- Comment pouvez-vous réutiliser cet exercice dans la réalisation du travail pratique 1 ? 
- Quels ajouts seraient nécessaires pour transformer cette liste en liste doublement chaînée ?
- Quels ajouts seraient nécessaires pour améliorer la performance d'un ajout ou d'une suppression en fin de liste ?