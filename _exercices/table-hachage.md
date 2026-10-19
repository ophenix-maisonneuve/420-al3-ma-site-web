---
layout: default
title: "Table de hachage"
nav_order: 8
has_toc: false
published: true
---

# Exercice : Implémentation d’une table de hachage

## Objectifs

- Comprendre le fonctionnement d’une **table associative** reposant sur une **fonction de hachage** (table de hachage).
- Implémenter les opérations de base : **ajout/mise à jour** (`ajouter`), **suppression** (`supprimer`), **recherche** (`contient`), **affichage par seau** (`afficherParSeau`).
- Observer la gestion des **collisions par chaînage séparé**.

{: .highlight}
> En moyenne, avec une bonne fonction de hachage et un facteur de charge maîtrisé, `ajouter/contient/supprimer` sont proches de **O(1)** amorti ; le **pire cas** est **O(n)** lorsque les collisions s’accumulent.

---

## Contexte

On vous fournit une classe `TableHachage` où :
- `ajouter(String cle, Object valeur)`, `contient(String cle)`, `get(String cle)` et `toString()` sont **déjà implémentées**
- `findEntry(String cle)` et `supprimer(String cle)` sont **à implémenter** et lèvent `UnsupportedOperationException`
- En bonus, `resize()` est **à implémenter** lorsque le facteur de charge de la table est dépassé.

---

## Étapes préparatoires

### 1. Clonez le dépôt de l'exercice

```bash
git clone git@github.com:ophenix-420-930-ma-24636/table-hachage.git
```
ou
```bash
git clone https://github.com/ophenix-420-930-ma-24636/table-hachage.git
```

### 2. Lancez le projet Java
```bash
mvn clean package
java -jar target/table-hachage-1.0-SNAPSHOT.jar
```
ou
```bash
mvn clean compile exec:java
```
ou directement à partir de votre IDE.

Familiarisez-vous avec le menu, qui vous permet déjà d'ajouter des éléments dans la table.

---

## Questions

### 1. Étudiez les classes `Entry` et `TableHachage`
- Expliquez le rôle de `indexFor(String cle)`.
- À quoi sert le masquage du bit de signe ?
- À quoi sert l'opérateur **modulo** avec la taille de la table

### 2. Étudiez la classe `TableHachage`
- Expliquez comment `ajouter` distingue **mise à jour** d'un élément existant vs **insertion** d'un nouvel élément.

### 3. Implémentez la recherche dans la table.
- Implémentez la méthode  `findEntry(String cle)` :
- Validez depuis le menu : l'implémentation de cette méthode aura rendu les méthodes `get(String cle)` et `contient(String cle)` disponibles
   - Tester les cas présent/absent et valeur `null`.

{: .highlight}
> Cette méthode permet ensuite l'implémentation facile de `containsKey` et `get`.

### 4. Implémentez la suppression dans la table.
- Implémentez `Object supprimer(String cle)` qui **retourne l’ancienne valeur** si supprimée, sinon `null`.
   - Gérez les cas : **tête**, **milieu/fin** de chaîne, **clé absente**.
- Quelle est la complexité grand O de la suppression dans la majorité des cas ? Dans le pire cas ?

{: .astuce}
> Pour l'implémentation, utilisez deux références `prev` / `curr` et mettez à jour le lien avec `setNext`.

### 5. Exercice Bonus: Redimensionnement
- Ajoutez et implémentez la méthode `resize()`.
- Cette méthode doit **doubler** la taille du tableau et *re-hasher*.
- Appelez cette méthode dans `ajouter` lorsque `size > table.length * loadFactor`.

---

## Ressources
- Diagramme de **chaînage séparé** : [Hash Table](../assets/images/hash-table-chainage-simple.png)
- Synthèse **Hash table** (définitions, complexités) : [Hash Table](https://en.wikipedia.org/wiki/Hash_table)
